---
Description: Los controles flotantes de las barras de comandos proporcionan a los usuarios acceso en línea a las tareas más comunes de tu app.
title: Control flotante de la barra de comandos
label: Command bar flyout
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: ksulliv
dev-contact: llongley
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6b85177e5d3d0f4a2a37010ba9122861216a4b6b
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081960"
---
# <a name="command-bar-flyout"></a>Control flotante de la barra de comandos

El control flotante de la barra de comandos permite proporcionar a los usuarios un acceso sencillo a las tareas comunes al mostrar los comandos en una barra de herramientas flotante relacionada con un elemento en el lienzo de la interfaz de usuario.

![Un control flotante de la barra de comandos de texto expandido](images/command-bar-flyout-header.png)

Al igual que [CommandBar](app-bars.md), CommandBarFlyout tiene las propiedades **PrimaryCommands** y **SecondaryCommands**, que puedes utilizar para agregar comandos. Puedes colocar los comandos en una de las colecciones o en ambas. La forma y el momento en que se muestran los comandos principales y secundarios dependen del modo de presentación.

El control flotante de la barra de comandos tiene dos modos de presentación: *contraído* y *expandido*.

- En el modo contraído, se muestran solo los comandos principales. Si el control flotante de la barra de comandos tiene comandos principales y secundarios, se muestra un botón "más", que se representa mediante unos puntos suspensivos \[***\]. Esto permite al usuario obtener acceso a los comandos secundarios al realizar la transición al modo expandido.
- En el modo expandido, se muestran los comandos principales y secundarios. (Si el control tiene únicamente elementos secundarios, estos se muestran de un modo parecido al control MenuFlyout).

**Obtención de la biblioteca de la interfaz de usuario de Windows**

|  |  |
| - | - |
| ![Logotipo de WinUI](images/winui-logo-64x64.png) | El control **CommandBarFlyout** se incluye como parte de la biblioteca de interfaz de usuario de Windows, un paquete NuGet que contiene nuevos controles y características de interfaz de usuario destinados a aplicaciones para UWP. Para obtener más información e instrucciones sobre la instalación, consulta el artículo [Windows UI Library ](https://docs.microsoft.com/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows). |

>**API de la biblioteca de interfaz de usuario de Windows**: [Clase CommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout), [clase TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)
>
>**API de plataforma**: [Clase CommandBarFlyout](/uwp/api/windows.ui.xaml.controls.commandbarflyout), [clase TextCommandBarFlyout](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout), [clase AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton), [clase AppBarToggleButton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [clase AppBarSeparator](/uwp/api/windows.ui.xaml.controls.appbarseparator)
>
> CommandBarFlyout requiere Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o posterior, o la [Biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Utiliza el control CommandBarFlyout para mostrar una colección de comandos para el usuario, como botones y elementos de menú, en el contexto de un elemento en el lienzo de la aplicación.

TextCommandBarFlyout muestra los comandos de texto en los controles TextBox, TextBlock, RichEditBox, RichTextBlock y PasswordBox. Los comandos se configuran correctamente de forma automática a la selección de texto actual. Usa un elemento CommandBarFlyout para sustituir los comandos de texto predeterminados en los controles de texto.

Para mostrar comandos contextuales en elementos de lista sigue las instrucciones de [Comandos contextuales para colecciones y listas](collection-commanding.md).

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout frente a MenuFlyout

Para mostrar los comandos en un menú contextual, puedes usar CommandBarFlyout o MenuFlyout. Se recomienda CommandBarFlyout porque proporciona más funcionalidad que MenuFlyout. Puedes usar CommandBarFlyout con solo comandos secundarios para obtener el comportamiento y aspecto de un elemento MenuFlyout o usar el control flotante de la barra de comandos completa con los comandos principales y secundarios.

> Para obtener información relacionada, consulta [Controles flotantes](../controls-and-patterns/dialogs-and-flyouts/flyouts.md), [Menús y menús contextuales](menus.md), y [Barras de comandos](app-bars.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la app <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/CommandBarFlyout">abrir la app y ver CommandBarFlyout en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>Invocación proactiva frente a reactiva

Normalmente, hay dos maneras de invocar un control flotante o un menú que está asociado con un elemento en el lienzo de la interfaz de usuario: _invocación proactiva_ e _invocación reactiva_.

En la invocación proactiva, los comandos aparecen automáticamente cuando el usuario interactúa con el elemento con el que se asocian. Por ejemplo, los comandos de formato de texto podrían aparecer cuando el usuario selecciona texto en un cuadro de texto. En este caso, el control flotante de la barra de comandos no recibe el foco. En su lugar, presenta los comandos pertinentes cerca del elemento con el que el usuario está interactuando. Si el usuario no interactúa con los comandos, se desestiman.

En la invocación reactiva, se muestran los comandos en respuesta a una acción explícita del usuario para solicitar los comandos como, por ejemplo, hacer clic con el botón derecho. Esto se corresponde con el concepto tradicional de un [menú contextual](menus.md).

Puedes usar CommandBarFlyout de ambas formas o incluso como una combinación de las dos.

## <a name="create-a-command-bar-flyout"></a>Creación de un control flotante de la barra de comandos

En este ejemplo se muestra cómo crear un control flotante de la barra de comandos y usarlo de forma proactiva y reactiva. Cuando se pulsa la imagen, el control flotante se muestra en el modo contraído. Cuando aparece como un menú contextual, el control flotante se muestra en el modo expandido. En ambos casos, el usuario puede expandir o contraer el control flotante una vez abierto.

![Ejemplo de un control flotante de la barra de comandos contraído](images/command-bar-flyout-img-collapsed.png)

> _Un control flotante de la barra de comandos contraído_

![Ejemplo de un control flotante de la barra de comandos expandido](images/command-bar-flyout-img-expanded.png)

> _Un control flotante de la barra de comandos expandido_

```xaml
<Grid>
    <Grid.Resources>
        <CommandBarFlyout x:Name="ImageCommandsFlyout">
            <AppBarButton Icon="OutlineStar" ToolTipService.ToolTip="Favorite"/>
            <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
            <AppBarButton Icon="Share" ToolTipService.ToolTip="Share"/>
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Select all"/>
                <AppBarButton Label="Delete" Icon="Delete"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/image1.png" Width="300"
           Tapped="Image_Tapped" FlyoutBase.AttachedFlyout="{x:Bind ImageCommandsFlyout}"
           ContextFlyout="{x:Bind ImageCommandsFlyout}"/>
</Grid>
```

```csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    var flyout = FlyoutBase.GetAttachedFlyout((FrameworkElement)sender);
    var options = new FlyoutShowOptions()
    {
        // Position shows the flyout next to the pointer.
        // "Transient" ShowMode makes the flyout open in its collapsed state.
        Position = e.GetPosition((FrameworkElement)sender),
        ShowMode = FlyoutShowMode.Transient
    };
    flyout?.ShowAt((FrameworkElement)sender, options);
}
```

### <a name="show-commands-proactively"></a>Muestra de los comandos de forma proactiva

Al mostrar los comandos contextuales de forma proactiva, solo se deben mostrar de manera predeterminada los comandos principales (se debe contraer el control flotante de la barra de comandos). Coloca los comandos más importantes en la colección principal de comandos y los comandos adicionales, que normalmente irían en un menú contextual, en la colección de comandos secundarios.

Para mostrar los comandos de forma proactiva, normalmente controlas el evento [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) o [Tapped](/uwp/api/windows.ui.xaml.uielement.tapped) con el fin de mostrar el control flotante de la barra de comandos. Establece el elemento [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) del control flotante en **Transient** o **TransientWithDismissOnPointerMoveAway** para abrir el control flotante en el modo contraído sin recibir el foco.

A partir de Windows 10 Insider Preview, los controles de texto tienen una propiedad **SelectionFlyout**. Cuando se asigna un control flotante a esta propiedad, se muestra automáticamente al seleccionar el texto.

### <a name="show-commands-reactively"></a>Muestra de los comandos de forma reactiva

Cuando se muestran comandos contextuales de forma reactiva, como un menú contextual, los comandos secundarios se muestran de manera predeterminada (el control flotante de la barra de comandos debe ampliarse). En este caso, el control flotante de la barra de comandos podría tener comandos principales y secundarios o solo secundarios.

Para mostrar los comandos en un menú contextual, por lo general se asigna el control flotante a la propiedad [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) de un elemento de la interfaz de usuario. De este modo, la apertura del control flotante la controla el elemento y no es necesario hacer nada más.

Si tú mismo controlas la presentación del control flotante (por ejemplo, en un evento [RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped)), establece el elemento [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) del control flotante en **Standard** para abrirlo en el modo expandido y obtener el foco.

> [!TIP]
> Para obtener más información acerca de las opciones cuando se muestre un control flotante y cómo controlar la colocación del control flotante, consulte [Controles flotantes](../controls-and-patterns/dialogs-and-flyouts/flyouts.md).

## <a name="commands-and-content"></a>Comandos y contenido

El control de CommandBarFlyout tiene 2 propiedades que puedes usar para agregar comandos y contenido: [PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands) y [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands).

De manera predeterminada, los elementos de las barras de comandos se agregan a la colección **PrimaryCommands**. Estos comandos se muestran en la barra de comandos y son visibles en los modos contraído y expandido. A diferencia de CommandBar, los comandos principales no se desbordan automáticamente a los comandos secundarios y podrían estar truncados.

También puedes agregar comandos a la colección **SecondaryCommands**. Los comandos secundarios se muestran en la parte del menú del control y solo son visibles en el modo expandido.

### <a name="app-bar-buttons"></a>Botones de la barra de la aplicación

Puedes rellenar las propiedades PrimaryCommands y SecondaryCommands directamente con los controles [AppBarButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton), [AppBarToggleButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarToggleButton) y [AppBarSeparator](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarSeparator).

Los controles de botón de la barra de la aplicación se caracterizan por un icono y una etiqueta de texto. Estos controles están optimizados para usarse en una barra de comandos y su aspecto cambia en función de si se muestra el control en la barra de comandos o el menú de desbordamiento.

- Los botones de la barra de la aplicación que se usan como comandos principales se muestran en la barra de comandos solo con el icono; la etiqueta de texto no se muestra. Se recomienda usar una información sobre herramientas para mostrar una descripción de texto del comando, tal como se muestra aquí.
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- Los botones de la barra de la aplicación que se usan como comandos secundarios se muestran en el menú, con la etiqueta y el icono visibles.

### <a name="other-content"></a>Otro contenido

Puedes agregar otros controles a un control flotante de la barra de comandos ajustándolos en un elemento AppBarElementContainer. Esto permite agregar controles, como [DropDownButton](buttons.md) o [SplitButton](buttons.md), o agregar contenedores como [StackPanel](buttons.md) para crear una interfaz de usuario más compleja.

Para poder agregarlos a las colecciones de comandos principales o secundarios de un control flotante de la barra de comandos, un elemento debe implementar la interfaz [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement). AppBarElementContainer es un contenedor que implementa esta interfaz para que puedas agregar un elemento a una barra de comandos, incluso si no implementa la propia interfaz.

En este caso, se usa un elemento AppBarElementContainer para agregar elementos adicionales a un control flotante de la barra de comandos. Se agrega un elemento SplitButton en los comandos principales para permitir la selección de colores. Se agrega un elemento StackPanel en los comandos secundarios para permitir un diseño más complejo para los controles de zoom.

> [!TIP]
> De manera predeterminada, los elementos diseñados para el lienzo de la aplicación podrían no resultar adecuados en una barra de comandos. Cuando se agrega un elemento mediante AppBarElementContainer, se deben seguir algunos pasos para hacer que el elemento coincida con otros elementos de la barra de comandos:
>
> - Invalida los pinceles predeterminados con [estilo ligero](/windows/uwp/design/controls-and-patterns/xaml-styles#lightweight-styling) para hacer que el fondo y el borde del elemento coincidan con los botones de la barra de la aplicación.
> - Ajusta el tamaño y la posición del elemento.
> - Ajusta los iconos en un Viewbox con un ancho y alto de 16px.

> [!NOTE]
> En este ejemplo se muestra solo la interfaz de usuario del control flotante de la barra de comandos, no implementa ninguno de los comandos que se muestran. Para obtener más información sobre la implementación de los comandos, consulte [Botones](buttons.md) y [Conceptos básicos del diseño de comando](../basics/commanding-basics.md).

![Un control flotante de la barra de comandos con un botón de expansión](images/command-bar-flyout-split-button.png)

> _Un control flotante de la barra de comandos contraído con un elemento SplitButton abierto_

![Un control flotante de la barra de comandos con una interfaz de usuario compleja](images/command-bar-flyout-custom-ui.png)

> _Un control flotante de la barra de comandos con una interfaz de usuario de zoom personalizada en el menú_


```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Cut" ToolTipService.ToolTip="Cut"/>
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    <AppBarButton Icon="Paste" ToolTipService.ToolTip="Paste"/>
    <!-- Alignment controls -->
    <AppBarElementContainer>
        <SplitButton ToolTipService.ToolTip="Alignment">
            <SplitButton.Resources>
                <!-- Override default brushes to make the SplitButton 
                     match other command bar elements. -->
                <Style TargetType="SplitButton">
                    <Setter Property="Height" Value="38"/>
                </Style>
                <SolidColorBrush x:Key="SplitButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="SplitButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrush" Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrushChecked"
                                 Color="Transparent"/>
            </SplitButton.Resources>
            <SplitButton.Content>
                <Viewbox Width="16" Height="16" Margin="0,2,0,0">
                    <SymbolIcon Symbol="AlignLeft"/>
                </Viewbox>
            </SplitButton.Content>
            <SplitButton.Flyout>
                <MenuFlyout>
                    <MenuFlyoutItem Icon="AlignLeft" Text="Align left"/>
                    <MenuFlyoutItem Icon="AlignCenter" Text="Center"/>
                    <MenuFlyoutItem Icon="AlignRight" Text="Align right"/>
                </MenuFlyout>
            </SplitButton.Flyout>
        </SplitButton>
    </AppBarElementContainer>
    <!-- end Alignment controls -->
    <CommandBarFlyout.SecondaryCommands>
        <!-- Zoom controls -->
        <AppBarElementContainer>
            <AppBarElementContainer.Resources>
                <!-- Override default brushes to make the Buttons 
                     match other command bar elements. -->
                <SolidColorBrush x:Key="ButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>
                <SolidColorBrush x:Key="ButtonBorderBrush"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushChecked"
                                 Color="Transparent"/>
                <Style TargetType="TextBlock">
                    <Setter Property="VerticalAlignment" Value="Center"/>
                </Style>
                <Style TargetType="Button">
                    <Setter Property="Height" Value="40"/>
                    <Setter Property="Width" Value="40"/>
                </Style>
            </AppBarElementContainer.Resources>
            <Grid Margin="12,-4">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="76"/>
                    <ColumnDefinition Width="Auto"/>
                </Grid.ColumnDefinitions>
                <Viewbox Width="16" Height="16" Margin="0,2,0,0">
                    <SymbolIcon Symbol="Zoom"/>
                </Viewbox>
                <TextBlock Text="Zoom" Margin="10,0,0,0" Grid.Column="1"/>
                <StackPanel Orientation="Horizontal" Grid.Column="2">
                    <Button ToolTipService.ToolTip="Zoom out">
                        <Viewbox Width="16" Height="16">
                            <SymbolIcon Symbol="ZoomOut"/>
                        </Viewbox>
                    </Button>
                    <TextBlock Text="50%" Width="40"
                               HorizontalTextAlignment="Center"/>
                    <Button ToolTipService.ToolTip="Zoom in">
                        <Viewbox Width="16" Height="16">
                            <SymbolIcon Symbol="ZoomIn"/>
                        </Viewbox>
                    </Button>
                </StackPanel>
            </Grid>
        </AppBarElementContainer>
        <!-- end Zoom controls -->
        <AppBarSeparator/>
        <AppBarButton Label="Undo" Icon="Undo"/>
        <AppBarButton Label="Redo" Icon="Redo"/>
        <AppBarButton Label="Select all" Icon="SelectAll"/>
    </CommandBarFlyout.SecondaryCommands>
</CommandBarFlyout>
```

## <a name="create-a-context-menu-with-secondary-commands-only"></a>Creación de un menú contextual solo con comandos secundarios

Puedes usar un elemento CommandBarFlyout con solo comandos secundarios como un [menú contextual](menus.md), en lugar de un elemento MenuFlyout.

![Un control flotante de la barra de comandos con solo comandos secundarios](images/command-bar-flyout-context-menu.png)

> _Control flotante de la barra de comandos como un menú contextual_

```xaml
<Grid>
    <Grid.Resources>
        <!-- A command bar flyout with only secondary commands. -->
        <CommandBarFlyout x:Name="ContextMenu">
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Copy" Icon="Copy"/>
                <AppBarButton Label="Save" Icon="Save"/>
                <AppBarButton Label="Print" Icon="Print"/>
                <AppBarSeparator />
                <AppBarButton Label="Properties"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/image1.png" Width="300"
           ContextFlyout="{x:Bind ContextMenu}"/>
</Grid>
```

También puedes usar un elemento CommandBarFlyout con un elemento DropDownButton para crear un menú estándar.

![Un control flotante de la barra de comandos a modo de menú de botón desplegable](images/command-bar-flyout-dropdown.png)

> _Un menú de botón desplegable en un control flotante de la barra de comandos_

```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Placeholder"/>
    <AppBarElementContainer>
        <DropDownButton Content="Mail">
            <DropDownButton.Resources>
                <!-- Override default brushes to make the DropDownButton 
                     match other command bar elements. -->
                <Style TargetType="DropDownButton">
                    <Setter Property="Height" Value="38"/>
                </Style>
                <SolidColorBrush x:Key="ButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>

                <SolidColorBrush x:Key="ButtonBorderBrush"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushChecked"
                                 Color="Transparent"/>
            </DropDownButton.Resources>
            <DropDownButton.Flyout>
                <CommandBarFlyout Placement="BottomEdgeAlignedLeft">
                    <CommandBarFlyout.SecondaryCommands>
                        <AppBarButton Icon="MailReply" Label="Reply"/>
                        <AppBarButton Icon="MailReplyAll" Label="Reply all"/>
                        <AppBarButton Icon="MailForward" Label="Forward"/>
                    </CommandBarFlyout.SecondaryCommands>
                </CommandBarFlyout>
            </DropDownButton.Flyout>
        </DropDownButton>
    </AppBarElementContainer>
    <AppBarButton Icon="Placeholder"/>
    <AppBarButton Icon="Placeholder"/>
</CommandBarFlyout>
```

## <a name="command-bar-flyouts-for-text-controls"></a>Controles flotantes de la barra de comandos para controles de texto

[TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) es un control flotante de la barra de comandos especializado que contiene comandos para editar texto. Cada control de texto muestra automáticamente TextCommandBarFlyout como un menú contextual (clic con el botón derecho) o cuando hay texto seleccionado. El control flotante de la barra de comandos de texto se adapta a la selección de texto para mostrar únicamente los comandos pertinentes.

![Un control flotante de la barra de comandos de texto contraído](images/command-bar-flyout-text-selection.png)

> _Un control flotante de la barra de comandos de texto en la selección de texto_

![Un control flotante de la barra de comandos de texto expandido](images/command-bar-flyout-text-full.png)

> _Un control flotante de la barra de comandos de texto expandido_


### <a name="available-commands"></a>Comandos disponibles

En esta tabla se muestran los comandos incluidos en un elemento TextCommandBarFlyout y cuándo se muestran.

| Comando | Mostrado... |
| ------- | -------- |
| Negrita | cuando el control de texto no es de solo lectura (solo RichEditBox). |
| Cursiva | cuando el control de texto no es de solo lectura (solo RichEditBox). |
| Subrayado | cuando el control de texto no es de solo lectura (solo RichEditBox). |
| Revisión | cuando IsSpellCheckEnabled es **true** y se selecciona el texto mal escrito. |
| Cortar | cuando el control de texto no es de solo lectura y se selecciona el texto. |
| Copiar | cuando se selecciona el texto. |
| Pegar | cuando el control de texto no es de solo lectura y el portapapeles tiene contenido. |
| Deshacer | cuando hay una acción que se puede deshacer. |
| Seleccionar todo | cuando se puede seleccionar el texto. |

### <a name="custom-text-command-bar-flyouts"></a>Controles flotantes de la barra de comandos de texto personalizado

No se puede personalizar TextCommandBarFlyout y cada control de texto lo administra automáticamente. Sin embargo, puedes sustituir el elemento TextCommandBarFlyout predeterminado con comandos personalizados.

- Para sustituir el valor de TextCommandBarFlyout predeterminado que se muestra en la selección de texto, puedes crear un elemento CommandBarFlyout personalizado (u otro tipo de control flotante) y asignarlo a la propiedad **SelectionFlyout**. Si estableces SelectionFlyout en **null**, no se muestran comandos en la selección.
- Para sustituir el valor de TextCommandBarFlyout predeterminado que se muestra como el menú contextual, asigna un elemento CommandBarFlyout personalizado (u otro tipo de control flotante) a la propiedad **ContextFlyout** en un control de texto. Si estableces ContextFlyout en **null**, se muestra el control flotante del menú que aparece en versiones anteriores del control de texto en lugar de TextCommandBarFlyout.

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.
- [Ejemplo de comandos XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

## <a name="related-articles"></a>Artículos relacionados

- [Conceptos básicos sobre el diseño de comandos de aplicaciones para la Plataforma universal de Windows (UWP)](../basics/commanding-basics.md)
- [Clase CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar)
