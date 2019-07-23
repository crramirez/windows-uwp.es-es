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
ms.openlocfilehash: 8f8d4172389fc2778fda4e335a29b3bae7d90fd0
ms.sourcegitcommit: 5fcd3a595efd3686009505602c34e10163fd0aa5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67558764"
---
# <a name="buttons"></a>Botones

Un botón ofrece al usuario una forma de desencadenar una acción inmediata. Algunos de los botones están especializados en determinadas tareas, como navegación, acciones repetidas o presentar los menús.

![Ejemplo de botones](images/controls/button.png)

El marco [XAML](../../xaml-platform/xaml-overview.md) proporciona un control de botón estándar, así como varios controles de botón especializados.

Control | Descripción
------- | -----------
[Button](/uwp/api/windows.ui.xaml.controls.button) | Un botón que inicia una acción inmediata. Puede usarse con un evento [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) o un enlace [Command](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command).
[RepeatButton](/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton) | Un botón que genera un evento [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) continuamente mientras se presiona.
[HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) | Un botón al que se le aplica el estilo de un hipervínculo y se usa para navegación. Para obtener más información sobre los hipervínculos, consulta [Hipervínculos](hyperlinks.md).
[DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) | Un botón con un botón de contenido adicional para abrir un control flotante adjunto.
[SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) | Un botón con dos lados. Uno de los lados inicia una acción y el otro abre un menú.
[ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) | Un botón de alternancia con dos lados. Un lado alterna entre activado/desactivado y el otro abre un menú.

| **Obtención de la biblioteca de la interfaz de usuario de Windows** |
| - |
| Los controles **DropDownButton**, **SplitButton** y **ToggleSplitButton** se incluyen como parte de la biblioteca de interfaz de usuario de Windows, un paquete NuGet que contiene nuevos controles y características de interfaz de usuario destinados a aplicaciones para UWP. Para obtener más información e instrucciones sobre la instalación, consulta el artículo [Windows UI Library ](https://docs.microsoft.com/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows). |

| **API de plataforma** | **API de la biblioteca de la interfaz de usuario de Windows** |
| - | - |
| [Evento Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)<br/> [Propiedad Command](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command) | [Clase DropDownButton](/uwp/api/microsoft.ui.xaml.controls.dropdownbutton)<br/> [Clase SplitButton](/uwp/api/microsoft.ui.xaml.controls.splitbutton)<br/> [Clase ToggleSplitButton](/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton) |

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa un control **Button** para permitir al usuario iniciar una acción inmediata, como enviar un formulario.

No uses un control [Button](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) cuando la acción sea navegar a otra página. En su lugar, usa un control **HyperlinkButton**. Para obtener más información sobre los hipervínculos, consulta [Hipervínculos](hyperlinks.md).

> [!IMPORTANT]
> Para la navegación por asistentes, usa los botones etiquetados *Atrás* y *Siguiente*. Para otros tipos de navegación hacia atrás o a un nivel superior, usa un [botón Atrás](../basics/navigation-history-and-backwards-navigation.md).

Usa un control **RepeatButton** cuando el usuario pueda querer desencadenar una acción varias veces. Por ejemplo, usa un control **RepeatButton** para aumentar o disminuir un valor en un contador.

Usa un control **DropDownButton** cuando el botón tenga un control flotante que con más opciones. El botón de contenido adicional predeterminado proporciona una indicación visual de que el botón incluye un control flotante.

Usa un control **SplitButton** cuando quieras que el usuario pueda iniciar una acción inmediata o elegir opciones adicionales por separado.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/Button">abrir la aplicación y ver el botón en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

En este ejemplo se usan dos botones, **Permitir** y **Bloquear**, en un cuadro de diálogo en el que se solicita acceso a la ubicación.

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

Controlar el evento [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click).

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

Cuando se pulsa un control **Button** con un dedo o un lápiz, o se presiona el botón primario del mouse mientras el puntero está sobre él, el botón genera el evento [Click](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). Si un botón tiene el foco del teclado, al presionar la tecla ENTRAR o la barra espaciadora, también se genera el evento **Click**.

Por lo general, no se pueden manipular los eventos [PointerPressed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed) de nivel bajo en un objeto **Button** porque tiene el comportamiento **Click** en su lugar. Para obtener más información, consulta el tema de [introducción a los eventos y eventos enrutados](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview).

Se puede cambiar la forma en que un botón genera el evento **Click** cambiando la propiedad [ClickMode](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.clickmode). El valor predeterminado de **ClickMode** es **Release**, pero también se puede establecer el valor **ClickMode** de un botón en **Hover** o **Press**. Si el valor de **ClickMode** es **Hover**, el evento **Click** no puede generarse mediante el teclado o la función táctil.


### <a name="button-content"></a>Contenido de Button

**Button** es un control de contenido de la clase [ContentControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentControl). Su propiedad de contenido XAML es [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.content), que habilita una sintaxis como esta para XAML: `<Button>A button's content</Button>`. Puedes establecer cualquier objeto como contenido del botón. Si el contenido es un objeto [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement), se representa en el botón. Si el contenido es otro tipo de objeto, su representación de cadena se muestra en el botón.

El contenido de un botón suele ser texto. Al diseñar dicho texto, ten en cuenta las siguientes recomendaciones:

-  Usa un texto conciso, específico y descriptivo que explique claramente la acción que realiza el botón. Por lo general, el texto del botón es una única palabra, un verbo.

-  Usa la fuente predeterminada, a menos que las directrices de marca indiquen lo contrario.

-  Para un texto más corto, evita los botones de comando estrechos usando un ancho de botón mínimo de 120 píxeles.

- Para un texto más largo, evita los botones de comando anchos limitando el texto a una longitud máxima de 26 caracteres.

-  Si el texto del botón es dinámico, (es decir, [localizado](../globalizing/globalizing-portal.md)), ten en cuenta cómo cambiará el tamaño del botón y qué ocurrirá con los controles cercanos.

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
<td> <b>Opción 2:</b><br> Aumenta el alto del botón y ajusta el texto. </td>
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

Un control [RepeatButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton) es un botón que genera eventos [Click](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) repetidamente desde que se presiona hasta que se suelta. Establece la propiedad [Delay](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton.delay) para especificar el tiempo que el control **RepeatButton** espera tras presionarlo, antes de que empiece a repetir la acción de clic. Establece la propiedad [Interval](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton.interval) para especificar el tiempo entre las repeticiones de la acción de clic. Los tiempos de ambas propiedades se especifican en milisegundos.

En el siguiente ejemplo se muestran dos controles **RepeatButton** ,cuyos respectivos eventos **Click** se usan para aumentar o disminuir el valor mostrado en un bloque de texto.

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

## <a name="create-a-drop-down-button"></a>Creación de un botón desplegable

> El control **DropDownButton** requiere la [Biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/) o Windows 10, versión 1809 (SDK 17763) o posterior. Para descargar el SDK más reciente, consulta [SDK de Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk). Para descargar un SDK anterior, consulta [Windows SDK y el archivo del emulador](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive).

Un elemento [DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) es un botón que muestra un botón de contenido adicional como un indicador visual de que tiene un control flotante asociado con más opciones. Tiene el mismo comportamiento que un control **Button** estándar con un control flotante; solo la apariencia es distinta.

El botón desplegable hereda el evento **Click**, pero normalmente no se usa. En su lugar, se usa la propiedad **Flyout** para adjuntar un control flotante e invocar acciones mediante las opciones de menú del control flotante. El control flotante se abre automáticamente cuando se hace clic en el botón.
No olvides especificar la propiedad [Placement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement) del control flotante para garantizar la ubicación elegida en relación con el botón. Es posible que el algoritmo de selección de ubicación predeterminado no produzca la ubicación elegida en todas las situaciones.

> [!TIP]
> Para obtener más información sobre los controles flotantes, consulta [Menús y menús contextuales](menus.md).

### <a name="example---drop-down-button"></a>Ejemplo: botón desplegable

En este ejemplo se muestra cómo crear un botón desplegable con un control flotante que tiene comandos para la alineación de párrafos en un control **RichEditBox**. (Para obtener más información y código, consulta [Cuadro de edición enriquecido](rich-edit-box.md)).

![Un botón desplegable con comandos de alineación](images/drop-down-button-align.png)

```xaml
<DropDownButton ToolTipService.ToolTip="Alignment">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="14" Text="&#xE8E4;"/>
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

## <a name="create-a-split-button"></a>Creación de un botón de expansión

 > [!IMPORTANT]
 > El control **SplitButton** requiere la [Biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/) o Windows 10, versión 1809 (SDK 17763) o posterior. Para descargar el SDK más reciente, consulta [SDK de Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk). Para descargar un SDK anterior, consulta [Windows SDK y el archivo del emulador](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive).

Un control [SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) tiene dos partes que se pueden invocar por separado. Una parte se comporta como un botón estándar e invoca una acción inmediata. La otra parte invoca un control flotante que contiene opciones adicionales entre las que puede elegir el usuario.

> [!NOTE]
> Cuando se invoca con función táctil, el botón de expansión se comporta como un botón desplegable; las dos mitades del botón invocan el control flotante. Con otros métodos de entrada, un usuario puede invocar cualquier mitad del botón por separado.

El comportamiento habitual de un botón de expansión es el siguiente:

- Cuando el usuario hace clic en la parte de botón, controla el evento **Click** para invocar la opción que está actualmente seleccionada en el menú desplegable.

- Cuando se abra el menú desplegable, controla la invocación de los elementos del menú para cambiar la opción que está seleccionada y luego invócala. Es importante invocar el elemento del control flotante, ya que el evento **Click** de botón no se produce cuando se usa la función táctil.

> [!TIP]
> Hay muchas maneras de colocar los elementos en el menú desplegable y controlar su invocación. Si usas un control **ListView** o **GridView**, una manera es controlar el evento **SelectionChanged**. Si haces esto, establece [SingleSelectionFollowsFocus](/uwp/api/windows.ui.xaml.controls.listviewbase.singleselectionfollowsfocus) en **false**. Esto permite a los usuarios navegar por las opciones mediante un teclado sin invocar el elemento en cada cambio.

### <a name="example---split-button"></a>Ejemplo: botón de expansión

En este ejemplo se muestra cómo crear un botón de expansión que se usa para cambiar el color de primer plano del texto seleccionado en un control **RichEditBox**. (Para obtener más información y código, consulta [Cuadro de edición enriquecido](rich-edit-box.md)).
El control flotante de los botones de expansión usa [BottomEdgeAlignedLeft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutplacementmode) como valor predeterminado para su propiedad [Placement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement). No se puede invalidar este valor.

![Botón de expansión para seleccionar el color de primer plano](images/split-button-rtb.png)

```xaml
<SplitButton ToolTipService.ToolTip="Foreground color"
             Click="BrushButtonClick">
    <Border x:Name="SelectedColorBorder" Width="20" Height="20"/>
    <SplitButton.Flyout>
        <Flyout x:Name="BrushFlyout">
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

## <a name="create-a-toggle-split-button"></a>Creación de un botón de expansión de alternancia

> [!NOTE]
> El control **ToggleSplitButton** requiere la [Biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/) o Windows 10, versión 1809 (SDK 17763) o posterior. Para descargar el SDK más reciente, consulta [SDK de Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk). Para descargar un SDK anterior, consulta [Windows SDK y el archivo del emulador](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive).

Un control [ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) tiene dos partes que se pueden invocar por separado. Una parte se comporta como un botón de alternancia que se puede activar o desactivar. La otra parte invoca un control flotante que contiene opciones adicionales entre las que puede elegir el usuario.

Normalmente, un botón de expansión de alternancia se usa para habilitar o deshabilitar una característica cuando esta tiene varias opciones entre las que puede elegir el usuario. Por ejemplo, en un editor de documentos, se podría usar para activar o desactivar listas, mientras que el menú desplegable se usa para elegir el estilo de la lista.

> [!NOTE]
> Cuando se invoca con función táctil, el botón de expansión de alternancia se comporta como un botón desplegable. Con otros métodos de entrada, un usuario puede alternar e invocar las dos mitades del botón por separado. Con función táctil, ambas mitades del botón invocan el control flotante. Por lo tanto, debes incluir una opción en el contenido del control flotante para activar o desactivar el botón.


### <a name="differences-with-togglebutton"></a>Diferencias con ToggleButton

A diferencia de [ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton), **ToggleSplitButton** no tiene un estado indeterminado. Como resultado, debes tener en cuenta estas diferencias:

- El control **ToggleSplitButton** no tiene una propiedad **IsThreeState** ni un evento **Indeterminate**.
- La propiedad [ToggleSplitButton.IsChecked](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischecked) es simplemente un valor booleano, no un valor **Nullable<bool>** .
- **ToggleSplitButton** solo tiene el evento [IsCheckedChanged](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischeckedchanged); no tiene eventos **Checked** y **Unchecked** independientes.


### <a name="example---toggle-split-button"></a>Ejemplo: botón de expansión de alternancia

En el ejemplo siguiente se muestra cómo un botón de expansión de alternancia se podría usar para activar o desactivar el formato de lista y cambiar el estilo de la lista, en un control **RichEditBox**. (Para obtener más información y código, consulta [Cuadro de edición enriquecido](rich-edit-box.md)).
El control flotante del botón de expansión de alternancia usa [BottomEdgeAlignedLeft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutplacementmode) como valor predeterminado para su propiedad [Placement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement). No se puede invalidar este valor.

![Un botón de expansión de alternancia para seleccionar los estilos de lista](images/toggle-split-button-open.png)

```xaml
<ToggleSplitButton x:Name="ListButton"
                   ToolTipService.ToolTip="List style"
                   Click="ListButton_Click"
                   IsCheckedChanged="ListStyleButton_IsCheckedChanged">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="14" Text="&#xE8FD;"/>
    <ToggleSplitButton.Flyout>
        <Flyout>
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

- Expón solo uno o dos botones al usuario a la vez. Por ejemplo, **Aceptar** y **Cancelar**. Si necesitas exponer más acciones al usuario, considera la posibilidad de usar [casillas](checkbox.md) o [botones de radio](radio-button.md) con los que el usuario pueda seleccionar acciones, con un único botón de comando para desencadenar estas acciones.

- En el caso de las acciones que deban estar disponibles en varias páginas de la aplicación, en vez de duplicar el mismo botón en varias páginas, piensa en utilizar una [barra de la aplicación inferior](app-bars.md).


### <a name="recommended-single-button-layout"></a>Diseño recomendado de botón único

Si el diseño requiere un único botón, debe estar alineado a la izquierda o a la derecha en función de su contexto de contenedor.

  - Los cuadros de diálogo con un único botón deben **alinear a la derecha** el botón. Si el cuadro de diálogo contiene solo un botón, asegúrate de que el botón realiza la acción segura y no destructiva. Si usas la clase [ContentDialog](dialogs.md) y especificas un único botón, se alineará automáticamente a la derecha.

    ![Un botón dentro de un cuadro de diálogo](images/pushbutton_doc_dialog.png)

  - Si el botón aparece dentro de una interfaz de usuario de contenedor (por ejemplo, dentro de una notificación del sistema, un control flotante o un elemento de vista de lista), debes **alinear a la derecha** el botón dentro del contenedor.

    ![Un botón dentro de un contenedor](images/pushbutton_doc_container.png)

  - En las páginas que contengan un solo botón (por ejemplo, un botón **Aplicar** en la parte inferior de una página de configuración), debes **alinear a la izquierda** el botón. Así se garantiza que el botón se alinea con el resto del contenido de la página.

    ![Un botón de una página](images/pushbutton_doc_page.png)


## <a name="back-buttons"></a>Botones Atrás

El botón Atrás es un elemento de la interfaz de usuario proporcionado por el sistema que permite la navegación hacia atrás a través de la pila de retroceso o el historial de navegación del usuario. No es necesario que crees tu propio botón Atrás, pero es posible que debas realizar algunas acciones para permitir que la experiencia de navegación hacia atrás resulte adecuada. Para obtener más información, consulta el artículo [Historial de navegación y navegación hacia atrás para las aplicaciones para UWP](../basics/navigation-history-and-backwards-navigation.md).


## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): En este ejemplo se muestran todos los controles XAML en un formato interactivo.


## <a name="related-articles"></a>Artículos relacionados

- [Clase Button](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button)
- [Botones de radio](radio-button.md)
- [Casillas](checkbox.md)
- [Modificadores para alternar](toggles.md)
