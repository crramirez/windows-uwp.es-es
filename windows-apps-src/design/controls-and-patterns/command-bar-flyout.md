---
Description: Menús emergentes de barra de comandos proporcionar a los usuarios acceso en línea a las tareas más comunes de la aplicación.
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
ms.openlocfilehash: cb87bea001492e39a0f60b96f884db70b5bd28ad
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592530"
---
# <a name="command-bar-flyout"></a>Control flotante de la barra de comandos

El control flotante de barra de comandos le permite proporcionar a los usuarios un acceso sencillo a las tareas comunes por mostrar los comandos en la barra de herramientas flotante relacionadas con un elemento en el lienzo de la interfaz de usuario.

![Un control flotante barra de comandos de texto expandido](images/command-bar-flyout-header.png)

> CommandBarFlyout requiere Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o posterior, o el [biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

> - **API de plataforma**: [Clase CommandBarFlyout](/uwp/api/windows.ui.xaml.controls.commandbarflyout), [TextCommandBarFlyout clase](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout), [AppBarButton clase](/uwp/api/windows.ui.xaml.controls.appbarbutton), [AppBarToggleButton clase](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [ Clase AppBarSeparator](/uwp/api/windows.ui.xaml.controls.appbarseparator)
>- **API de biblioteca de interfaz de usuario de Windows**: [Clase CommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout), [TextCommandBarFlyout clase](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)

Al igual que [CommandBar](app-bars.md), tiene CommandBarFlyout **PrimaryCommands** y **SecondaryCommands** propiedades que puede utilizar para agregar los comandos. Puede colocar los comandos en la colección, o ambos. Depende de cuándo y cómo se muestran los comandos principales y secundarios en el modo de presentación.

El control de barra de comandos flotante tiene dos modos de presentación: *contraído* y *expandido*.

- En el modo contraído, se muestran solo los comandos principales. Si su control de barra de comandos flotante tiene principal y secundaria comandos, un botón "más", que se representa mediante un botón de puntos suspensivos \[•••\], se muestra. Esto permite al usuario obtener acceso a los comandos secundarios al realizar la transición al modo expandido.
- En el modo expandido, se muestran los comandos principales y secundarios. (Si el control tiene únicamente los elementos secundarios, se muestran en forma similar al control MenuFlyout.)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Utilice el control CommandBarFlyout para mostrar una colección de comandos para el usuario, como botones y elementos de menú, en el contexto de un elemento en el lienzo de la aplicación.

La TextCommandBarFlyout muestra los comandos de texto en los controles TextBox, TextBlock, RichEditBox, RichTextBlock y PasswordBox. Los comandos se configuran automáticamente adecuadamente a la selección de texto actual. Utilice un CommandBarFlyout para reemplazar los comandos de texto de forma predeterminada en los controles de texto.

Para mostrar contextuales en elementos de lista de comandos sigan las instrucciones de [comandos contextuales para listas y colecciones](collection-commanding.md).

### <a name="commandbarflyout-vs-menuflyout"></a>Vs CommandBarFlyout MenuFlyout

Para mostrar los comandos en un menú contextual, puede usar CommandBarFlyout o MenuFlyout. Se recomienda CommandBarFlyout porque proporciona más funcionalidad que MenuFlyout. Puede usar CommandBarFlyout con solo los comandos secundarios para obtener el comportamiento y aspecto de un MenuFlyout o use la barra de comandos completa ventana flotante con comandos principales y secundarios.

> Para obtener información relacionada, consulte [Flyout](../controls-and-patterns/dialogs-and-flyouts/flyouts.md), [menús y menús contextuales](menus.md), y [barras de comandos](app-bars.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tiene la <strong style="font-weight: semi-bold">Galería de controles XAML</strong> aplicación instalada, haga clic aquí para <a href="xamlcontrolsgallery:/item/CommandBarFlyout">abra la aplicación y vea el CommandBarFlyout en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>Proactivo frente a reactivo invocación

Normalmente, hay dos maneras para invocar una ventana flotante o un menú que está asociado con un elemento en el lienzo de la interfaz de usuario: _invocación proactiva_ y _invocación reactivo_.

En la invocación proactiva, los comandos aparecen automáticamente cuando el usuario interactúa con el elemento que se asocian los comandos. Por ejemplo, los comandos de formato de texto podrían aparecer cuando el usuario selecciona texto en un cuadro de texto. En este caso, el control flotante de barra de comandos no tiene el foco. En su lugar, presenta los comandos pertinentes cerca del elemento que el usuario está interactuando con. Si el usuario no interactúa con los comandos, se desestiman.

En la invocación reactivo, se muestran los comandos en respuesta a una acción explícita del usuario para solicitar los comandos; Por ejemplo, un botón secundario. Esto se corresponde con el concepto tradicional de un [menú contextual](menus.md).

Puede usar el CommandBarFlyout en forma o incluso una combinación de ambos.

## <a name="create-a-command-bar-flyout"></a>Crear un control de barra de comandos flotante

En este ejemplo se muestra cómo crear un control de barra de comandos flotante y usarlo tanto proactiva como reactiva. Cuando se pulsa la imagen, se muestra el control flotante en su modo contraído. Cuando se muestra como un menú contextual, el control flotante se muestra en el modo expandido. En cualquier caso, el usuario puede expandir o contraer la ventana flotante después de abrirse.

![Ejemplo de un control flotante de barra de comandos contraído](images/command-bar-flyout-img-collapsed.png)

> _Un flotante de barra de comandos contraído_

![Ejemplo de un control de barra de comandos expandido flotante](images/command-bar-flyout-img-expanded.png)

> _Un control de barra de comandos expandido flotante_

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

### <a name="show-commands-proactively"></a>Mostrar los comandos de forma proactiva

Al mostrar los comandos contextuales de forma proactiva, se deben mostrar solo los comandos principales de forma predeterminada (se debe contraer el control de barra de comandos flotante). Coloque los comandos más importantes de la colección de comandos principal y los comandos adicionales que tradicionalmente entraban en un menú contextual en la colección de comandos secundarios.

Para mostrar los comandos de forma proactiva, que normalmente controla el [haga clic en](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) o [pulsado](/uwp/api/windows.ui.xaml.uielement.tapped) evento para mostrar la ventana flotante de barra de comandos. Establecer la ventana de flotante [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) a **transitorios** o **TransientWithDismissOnPointerMoveAway** para abrir la ventana flotante en su modo contraído sin tomar el foco.

A partir de Windows 10 Insider Preview, controles de texto tienen una **SelectionFlyout** propiedad. Cuando un control flotante se asigna a esta propiedad, se muestra automáticamente cuando se selecciona el texto.

### <a name="show-commands-reactively"></a>Mostrar los comandos de forma reactiva

Cuando reactiva, como un menú contextual muestra comandos contextuales, los comandos secundarios se muestran de forma predeterminada (debe ampliarse el control de barra de comandos flotante). En este caso, podría tener el control de barra de comandos flotante comandos principales y secundarios o sólo los comandos secundarios.

Para mostrar los comandos en un menú contextual, por lo general asignan la ventana flotante para el [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) propiedad de un elemento de interfaz de usuario. De este modo, abra la ventana flotante se controla mediante el elemento y no es necesario hacer nada más.

Si controla la presentación de la ventana flotante usted mismo (por ejemplo, en un [RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped) eventos), establecer la ventana de flotante [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) a **estándar** para abrir la ventana flotante en su modo expandido y tenga el foco.

> [!TIP]
> Para obtener más información acerca de las opciones cuando se muestre un control flotante y cómo controlar la colocación de la ventana flotante, vea [flotantes](../controls-and-patterns/dialogs-and-flyouts/flyouts.md).

## <a name="commands-and-content"></a>Comandos y contenido

El control CommandBarFlyout tiene 2 propiedades que puede utilizar para agregar comandos y el contenido: [PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands) y [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands).

De forma predeterminada, los elementos de las barras de comandos se agregan a la colección **PrimaryCommands**. Estos comandos se muestran en la barra de comandos y son visibles en los modos contraídos y expandidos. A diferencia de la barra de comandos, comandos principales no lo hace automáticamente desbordamiento a los comandos secundaria y podrían aparecer truncados.

También puede agregar comandos a la **SecondaryCommands** colección. Comandos secundarios se muestran en la parte del menú del control y solo son visibles en el modo expandido.

### <a name="app-bar-buttons"></a>Botones de la barra de la aplicación

Puede rellenar el PrimaryCommands y SecondaryCommands directamente con [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx), y [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) controles.

Los controles de botón de la barra de la aplicación se caracterizan por un icono y una etiqueta de texto. Estos controles están optimizados para su uso en una barra de comandos y su apariencia cambia dependiendo de si el control se muestra en la barra de comandos o en el menú de desbordamiento.

- Los botones de barra de aplicación sirve de los comandos principales se muestran en la barra de comandos con solo su icono; no se muestra la etiqueta de texto. Se recomienda usar una información sobre herramientas para mostrar una descripción de texto del comando, como se muestra aquí.
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- Los botones de barra de aplicación usan como comandos secundarios se muestran en el menú, con la etiqueta y el icono visible.

### <a name="other-content"></a>Otro contenido

Puede agregar otros controles a un control de barra de comandos flotante ajustándolas en un AppBarElementContainer. Esto le permite agregar controles, como [DropDownButton](buttons.md) o [SplitButton](buttons.md), o agregar como contenedores [StackPanel](buttons.md) para crear la interfaz de usuario más compleja.

Para agregarse a las colecciones de comando principal o secundaria de un control flotante de barra de comandos, debe implementar un elemento del [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement) interfaz. AppBarElementContainer es un contenedor que implementa esta interfaz para que pueda agregar un elemento a una barra de comandos, incluso si no implementa la interfaz de sí mismo.

En este caso, se usa un AppBarElementContainer para agregar elementos adicionales a un control flotante de barra de comandos. Un botón de división se agrega a los comandos principales para permitir la selección de colores. Se agrega un elemento StackPanel a los comandos secundarios para permitir que un diseño más complejo para los controles de zoom.

> [!TIP]
> De forma predeterminada, los elementos que se ha diseñado para el lienzo de la aplicación podrían no resultar adecuados en una barra de comandos. Cuando se agrega un elemento mediante AppBarElementContainer, hay algunos pasos que debe seguir para hacer que el elemento coincide con otros elementos de la barra de comandos:
>
> - Invalidar los pinceles de forma predeterminada con [estilo ligera](/windows/uwp/design/controls-and-patterns/xaml-styles#lightweight-styling) para hacer que el elemento en segundo plano y borde coinciden con los botones de barra de la aplicación.
> - Ajustar el tamaño y posición del elemento.
> - Ajuste los iconos en un Viewbox con un ancho y alto de 16px.

> [!NOTE]
> Este ejemplo muestra solo el comando barra control flotante de interfaz de usuario, no implementa cualquiera de los comandos que se muestran. Para obtener más información sobre la implementación de los comandos, consulte [botones](buttons.md) y [conceptos básicos del diseño de comando](../basics/commanding-basics.md).

![Un flotante de la barra de comandos con un botón de expansión](images/command-bar-flyout-split-button.png)

> _Un flotante de barra de comando contraído con un botón de división abierto_

![Un flotante de barra de comandos con la interfaz de usuario compleja](images/command-bar-flyout-custom-ui.png)

> _Un control de barra de comandos expandido flotante con la interfaz de usuario de zoom personalizado en el menú_


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

## <a name="create-a-context-menu-with-secondary-commands-only"></a>Crear un menú contextual con comandos secundarios solo

Puede usar un CommandBarFlyout con solo los comandos secundarios como un [menú contextual](menus.md), en lugar de un MenuFlyout.

![Un flotante de barra de comandos con comandos secundarios solo](images/command-bar-flyout-context-menu.png)

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

También puede usar un CommandBarFlyout con un DropDownButton para crear un menú estándar.

![Un control de barra de comandos flotante con como un menú desplegable del menú del botón](images/command-bar-flyout-dropdown.png)

> _Un menú desplegable del menú del botón en un control de barra de comandos flotante_

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

## <a name="command-bar-flyouts-for-text-controls"></a>Menús emergentes de la barra de comandos para los controles de texto

El [TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) es un control flotante de barra de comandos especializados que contiene comandos para editar texto. Cada control de texto, muestra automáticamente el TextCommandBarFlyout como un menú contextual (clic derecho), o cuando hay texto seleccionado. El control flotante barra de comandos de texto se adapta a la selección de texto para mostrar únicamente los comandos pertinentes.

![Un flotante de barra de comandos de texto contraído](images/command-bar-flyout-text-selection.png)

> _Un flotante de barra de comandos de texto en la selección de texto_

![Un control flotante barra de comandos de texto expandido](images/command-bar-flyout-text-full.png)

> _Un control flotante barra de comandos de texto expandido_


### <a name="available-commands"></a>Comandos disponibles

Esta tabla muestra los comandos que se incluyen en un TextCommandBarFlyout y, cuando se muestran.

| Comando | Se muestra... |
| ------- | -------- |
| Negrita | Cuando el control de texto no es de solo lectura (RichEditBox solo). |
| Cursiva | Cuando el control de texto no es de solo lectura (RichEditBox solo). |
| Subrayado | Cuando el control de texto no es de solo lectura (RichEditBox solo). |
| Corrección | Cuando es IsSpellCheckEnabled **true** y se selecciona el texto mal escrito. |
| Cortar | Cuando el control de texto no es de solo lectura y se selecciona el texto. |
| Copiar | Cuando se selecciona el texto. |
| Pegar | Cuando el control de texto no es de solo lectura y el Portapapeles tiene contenido. |
| Deshacer | Cuando hay una acción que se puede deshacer. |
| Seleccionar todos | cuándo se puede seleccionar el texto. |

### <a name="custom-text-command-bar-flyouts"></a>Menús emergentes de barra de comandos de texto personalizado

No se puede personalizar TextCommandBarFlyout y se administra automáticamente por cada control de texto. Sin embargo, puede reemplazar el valor predeterminado TextCommandBarFlyout con comandos personalizados.

- Para reemplazar el valor predeterminado TextCommandBarFlyout que se muestra en la selección de texto, puede crear un CommandBarFlyout personalizado (u otro tipo de control flotante) y asígnelo a la **SelectionFlyout** propiedad. Si establece SelectionFlyout en **null**, no hay comandos se muestran en la selección.
- Para reemplazar el valor predeterminado TextCommandBarFlyout que se muestra como el menú contextual, asignar un CommandBarFlyout personalizado (u otro tipo de control flotante) a la **ContextFlyout** propiedad en un control de texto. Si establece ContextFlyout en **null**, en el menú flotante se muestra en las versiones anteriores del texto del control se muestra en lugar de la TextCommandBarFlyout.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery): ve todos los controles XAML en un formato interactivo.
- [Ejemplo de comandos de XAML](https://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>Artículos relacionados

- [Conceptos básicos sobre el diseño de comandos de aplicaciones para la Plataforma universal de Windows (UWP)](../basics/commanding-basics.md)
- [Clase de barra de comandos](https://msdn.microsoft.com/library/windows/apps/dn279427)
