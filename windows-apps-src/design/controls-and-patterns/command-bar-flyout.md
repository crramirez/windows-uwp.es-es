---
author: jwmsft
Description: Command bar flyouts give users inline access to your app's most common tasks.
title: Control flotante de barra de comandos
label: Command bar flyout
template: detail.hbs
ms.author: jimwalk
ms.date: 10/2/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: abarlow
design-contact: ksulliv
dev-contact: llongley
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 95d99c41ff2679e3ef3e0471dd583fe78458922c
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "5919163"
---
# <a name="command-bar-flyout"></a>Control flotante de barra de comandos

El elemento de barra de comandos le permite proporcionar a los usuarios con fácil acceso a tareas comunes al mostrar comandos en una barra de herramientas flotante relacionado con un elemento en el lienzo de interfaz de usuario.

![Un flotante de barra de comandos de texto expandido](images/command-bar-flyout-header.png)

> CommandBarFlyout requiere Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o posterior, o en la [Biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

> - **API de la plataforma**: [clase CommandBarFlyout](/uwp/api/windows.ui.xaml.controls.commandbarflyout), [clase TextCommandBarFlyout](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout), [clase AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton), [clase AppBarToggleButton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [clase AppBarSeparator](/uwp/api/windows.ui.xaml.controls.appbarseparator)
>- **Biblioteca de API de Windows de interfaz de usuario**: [CommandBarFlyout (clase)](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout), [clase TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)

Al igual que [CommandBar](app-bars.md), CommandBarFlyout tiene propiedades **PrimaryCommands** y **SecondaryCommands** que puede utilizar para agregar los comandos. Puede colocar los comandos en la colección, o ambos. Cuándo y cómo se muestran los comandos primarios y secundarios depende del modo de presentación.

El elemento de barra de comandos tiene dos modos de visualización: *contraído* y *expandido*.

- En el modo contraído, aparecen sólo los comandos principales. Si la barra de comando barra flotante tiene primaria y secundaria comandos, un botón "ver más", que se representa mediante puntos suspensivos \ [•••\], se muestra. Esto permite al usuario obtener acceso a los comandos secundarios por su transición a modo expandido.
- En el modo expandido, se muestran los comandos primarios y secundarios. (Si el control tiene sólo los elementos secundarios, aparecen de forma similar al control MenuFlyout.)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Utilice el control de CommandBarFlyout para mostrar una colección de comandos para el usuario, como botones y elementos de menú, en el contexto de un elemento en el lienzo de la aplicación.

La TextCommandBarFlyout muestra los comandos de texto en los controles TextBox, TextBlock, RichEditBox, RichTextBlock y PasswordBox. Los comandos automáticamente están configurados correctamente para la selección de texto actual. Utilice un CommandBarFlyout para reemplazar los comandos de texto predeterminado en los controles de texto.

Para mostrar contextuales en elementos de la lista comandos sigan las instrucciones [contextuales para las colecciones y listas de comandos](collection-commanding.md).

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout vs MenuFlyout

Para mostrar los comandos de un menú contextual, puede utilizar CommandBarFlyout o MenuFlyout. Se recomienda CommandBarFlyout porque proporciona más funcionalidad que MenuFlyout. Puede utilizar CommandBarFlyout con sólo comandos secundarios para obtener el comportamiento y aspecto de una MenuFlyout o utilizar la barra de comando completo elementos flotantes con los comandos primarios y secundarios.

> Para obtener información relacionada, vea [menús emergentes](../controls-and-patterns/dialogs-and-flyouts/flyouts.md), [menús y menús contextuales](menus.md)y [barras de comandos](app-bars.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tiene la <strong style="font-weight: semi-bold">Galería de controles de XAML</strong> de la aplicación instalada, haga clic aquí para <a href="xamlcontrolsgallery:/item/CommandBarFlyout">Abrir la aplicación y ver el CommandBarFlyout en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>Proactivo frente a reactivo invocación

Normalmente existen dos formas invocar un menú que está asociado con un elemento en el lienzo de IU o flotante: _invocación proactivo_ y _reactivo invocación_.

En la invocación proactivo, comandos aparecen automáticamente cuando el usuario interactúa con el elemento que se asocian los comandos. Por ejemplo, los comandos de formato de texto pueden aparecer cuando el usuario selecciona texto en un cuadro de texto. En este caso, el elemento de barra de comandos no toma el foco. En su lugar, presenta los comandos pertinentes cerca del elemento que el usuario está interactuando con el. Si el usuario no interactúa con los comandos, se descarta.

En la invocación reactivo, los comandos se muestran en respuesta a una acción explícita del usuario para solicitar los comandos; Por ejemplo, un ratón. Esto corresponde al concepto tradicional de un [menú contextual](menus.md).

Puede utilizar el CommandBarFlyout en la forma, o incluso una mezcla de los dos.

## <a name="create-a-command-bar-flyout"></a>Crear un flotante de barra de comandos

En este ejemplo se muestra cómo crear un flotante de la barra de comando y utilizarlo tanto proactiva como reactiva. Cuando se puntea la imagen, el flotante se muestra en el modo contraído. Cuando se muestra como un menú contextual, el flotante se muestra en el modo expandido. En cualquier caso, el usuario puede expandir o contraer el elemento después de abrirlo.

![Ejemplo de un flotante de barra de comandos contraído](images/command-bar-flyout-img-collapsed.png)

> _Un flotante de barra de comandos contraído_

![Ejemplo de un emergente de barra de comando expandido](images/command-bar-flyout-img-expanded.png)

> _Un flotante de la barra de comando extendido_

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

Cuando se muestran comandos contextuales proactivamente, sólo los comandos principales deben mostrarse por defecto (el elemento de barra de comandos debe estar contraído). Coloque los comandos más importantes de la colección de comandos principal y los comandos adicionales que tradicionalmente entraría en un menú contextual en la colección de comandos secundario.

Para mostrar los comandos de forma proactiva, se suele controlar el evento de [roscados](/uwp/api/windows.ui.xaml.uielement.tapped) o [haga clic en](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) para mostrar el elemento de barra de comandos. Establezca del flotante [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) **transitorios** o **TransientWithDismissOnPointerMoveAway** para abrir el elemento en su modo contraído sin retirar el foco.

A partir de la vista previa de Windows 10 Insider, controles de texto tienen una propiedad **SelectionFlyout** . Cuando se asigna un flotante a esta propiedad, se muestra automáticamente cuando se selecciona texto.

### <a name="show-commands-reactively"></a>Mostrar los comandos de forma reactiva

Cuando se muestran comandos contextuales reactiva, como un menú contextual, se muestren los comandos secundarios (el elemento de barra de comandos debe ampliarse) de forma predeterminada. En este caso, el elemento de barra de comandos puede tener comandos primarios y secundarios, o sólo comandos secundarios.

Para mostrar los comandos de un menú de contexto, normalmente asigna el elemento a la propiedad [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) de un elemento de interfaz de usuario. De este modo, abrir el flotante se controla por el elemento, y no necesita hacer nada más.

Si se controla mostrando el flotante (por ejemplo, en un evento [RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped) ), establezca del flotante [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) en **estándar** para abrir la barra flotante en su modo expandido y asignarle el foco.

> [!TIP]
> Para obtener más información acerca de las opciones cuando mostrando un flotante y cómo controlar la ubicación del flotante, vea [menús emergentes](../controls-and-patterns/dialogs-and-flyouts/flyouts.md).

## <a name="commands-and-content"></a>Comandos y contenido

El control de CommandBarFlyout tiene 2 propiedades que puede utilizar para agregar comandos y contenido: [PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands) y [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands).

De forma predeterminada, los elementos de las barras de comandos se agregan a la colección **PrimaryCommands**. Estos comandos se muestran en la barra de comandos y son visibles en los modos contraídos y expandidos. A diferencia de CommandBar, comandos principales no lo hace automáticamente desbordamiento a los comandos secundarios y se truncarán.

También puede agregar comandos a la colección **SecondaryCommands** . Comandos secundarios se muestran en la parte del menú del control y sólo son visibles en el modo expandido.

### <a name="app-bar-buttons"></a>Botones de la barra de la aplicación

Puede rellenar el PrimaryCommands y el SecondaryCommands directamente con los controles de [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx)y [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) .

Los controles de botón de la barra de la aplicación se caracterizan por un icono y una etiqueta de texto. Estos controles están optimizados para su uso en una barra de comandos y su aspecto cambia dependiendo de si el control se muestra en la barra de comandos o en el menú de desbordamiento.

- Botones de la barra de la aplicación utilizados como comandos principales se muestran en la barra de comandos con sólo su icono; no se muestra la etiqueta de texto. Se recomienda utilizar un control tooltip para mostrar una descripción de texto del comando, como se muestra aquí.
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- Botones de la barra de la aplicación utilizados como comandos secundarios se muestran en el menú, con la etiqueta y el icono visible.

### <a name="other-content"></a>Otro contenido

Puede agregar otros controles a un flotante de barra de comandos y ajustarlas en un AppBarElementContainer. Esto permite agregar controles como [DropDownButton]() o [SplitButton]()o agregar contenedores como [StackPanel]() para crear la interfaz de usuario más compleja.

Para agregarse a las colecciones de comando primario o secundario de un emergente de barra de comandos, un elemento debe implementar la interfaz [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement) . AppBarElementContainer es un contenedor que implementa esta interfaz para que pueda agregar un elemento a una barra de comandos, incluso si no implementa la interfaz.

Aquí, un AppBarElementContainer se utiliza para agregar elementos adicionales a un flotante de barra de comandos. Los comandos principales para permitir la selección de colores se agrega un botón de división. Se agrega un elemento StackPanel a los comandos secundarios para permitir un diseño más complejo para los controles de zoom.

> [!TIP]
> De forma predeterminada, elementos diseñados para el lienzo app no sería adecuados en una barra de comandos. Cuando se agrega un elemento mediante AppBarElementContainer, hay algunos pasos que debe seguir para hacer que el elemento coincida con otros elementos de la barra de comandos:
>
> - Reemplazar el valor predeterminado de pinceles con [estilo ligero](/design/controls-and-patterns/xaml-styles#lightweight-styling) para hacer el fondo y el borde del elemento coincide con los botones de la barra de la aplicación.
> - Ajustar el tamaño y la posición del elemento.
> - Ajustar los iconos en un Viewbox con un ancho y alto de 16px.

> [!NOTE]
> Este ejemplo muestra sólo el elemento de barra de comandos IU, no implementar cualquiera de los comandos que se muestran. Para obtener más información acerca de cómo implementar los comandos, consulte [botones](buttons.md) y [conceptos básicos del diseño de comando](../basics/commanding-basics.md).

![Un flotante de barra de comandos con un botón de división](images/command-bar-flyout-split-button.png)

> _Un flotante barra de comando contraído con un SplitButton abierto_

![Un flotante de barra de comandos con la interfaz de usuario compleja](images/command-bar-flyout-custom-ui.png)

> _Un flotante de la barra de comando expandido con interfaz de usuario de zoom personalizado en el menú_


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

## <a name="create-a-context-menu-with-secondary-commands-only"></a>Crear un menú contextual con comandos secundarios sólo

Puede utilizar un CommandBarFlyout con sólo comandos secundarios como un [menú contextual](menus.md), en lugar de un MenuFlyout.

![Un flotante de barra de comandos con sólo comandos secundarios](images/command-bar-flyout-context-menu.png)

> _Comando barra flotante como un menú contextual_

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

También puede utilizar un CommandBarFlyout con un DropDownButton para crear un menú estándar.

![Un barra de comandos flotante con como un botón menú desplegable](images/command-bar-flyout-dropdown.png)

> _Un menú desplegable de botón de una barra de comandos elementos flotantes_

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

El [TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) es un flotante de barra de comandos especializados que contiene comandos de edición de texto. Cada control de texto muestra automáticamente el TextCommandBarFlyout como un menú contextual (botón secundario), o cuando se selecciona texto. La barra de comandos de texto elementos flotantes se adapta a la selección de texto para mostrar sólo los comandos pertinentes.

![Un flotante de barra de comandos de texto contraído](images/command-bar-flyout-text-selection.png)

> _Un flotante de barra de comandos de texto en la selección de texto_

![Un flotante de barra de comandos de texto expandido](images/command-bar-flyout-text-full.png)

> _Un flotante de barra de comandos de texto expandido_


### <a name="available-commands"></a>Comandos disponibles

Esta tabla muestra los comandos que se incluyen en un TextCommandBarFlyout, y cuando se muestran.

| Comando | Se muestra... |
| ------- | -------- |
| Negrita | Cuando el control de texto no es de sólo lectura (RichEditBox solamente). |
| Cursiva | Cuando el control de texto no es de sólo lectura (RichEditBox solamente). |
| Subrayado | Cuando el control de texto no es de sólo lectura (RichEditBox solamente). |
| Corrección | Cuando IsSpellCheckEnabled es **true** y mal escrito texto seleccionado. |
| Cortar | Cuando el control de texto no es de sólo lectura y se selecciona el texto. |
| Copiar | Cuando se selecciona texto. |
| Pegar | Cuando el control de texto no es de sólo lectura y el Portapapeles tiene contenido. |
| Deshacer | Cuando hay una acción que se puede deshacer. |
| Seleccionar todos | Cuando se puede seleccionar el texto. |

### <a name="custom-text-command-bar-flyouts"></a>Menús emergentes de barra de comandos de texto personalizado

TextCommandBarFlyout no se pueden personalizar y es administrado automáticamente por cada control de texto. Sin embargo, puede reemplazar el valor predeterminado TextCommandBarFlyout con comandos personalizados.

- Para reemplazar el valor predeterminado de TextCommandBarFlyout que se muestra en la selección de texto, puede crear una personalizada CommandBarFlyout (u otro tipo de elementos flotantes) y asignarlo a la propiedad **SelectionFlyout** . Si SelectionFlyout se establece en **null**, no hay comandos se muestran en la selección.
- Para reemplazar el valor predeterminado de TextCommandBarFlyout que se muestra como el menú de contexto, asignar un CommandBarFlyout personalizado (u otro tipo de elementos flotantes) a la propiedad **ContextFlyout** en un control de texto. Si ContextFlyout se establece en **null**, se muestra el elemento de menú que se muestra en las versiones anteriores del control de texto en lugar de la TextCommandBarFlyout.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.
- [Muestra de comandos XAML](http://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>Artículos relacionados

- [Conceptos básicos de diseño de los comandos para las aplicaciones para UWP](../basics/commanding-basics.md)
- [Clase CommandBar](https://msdn.microsoft.com/library/windows/apps/dn279427)
