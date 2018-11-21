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
ms.custom: RS5
ms.openlocfilehash: 4b92b74ba65a683143e273961ed25ce478a6e81f
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7553212"
---
# <a name="command-bar-flyout"></a>Control flotante de barra de comandos

El control flotante de barra de comandos te permite proporcionar a los usuarios acceso fácil a las tareas comunes que muestra los comandos en una barra de herramientas flotante relacionadas con un elemento en el lienzo de la interfaz de usuario.

![Un control flotante la barra de comandos de texto expandido](images/command-bar-flyout-header.png)

> CommandBarFlyout requiere Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o posterior, o en la [Biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

> - **API de la plataforma**: [clase CommandBarFlyout](/uwp/api/windows.ui.xaml.controls.commandbarflyout), [clase TextCommandBarFlyout](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout), [clase AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton), [clase AppBarToggleButton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [clase AppBarSeparator](/uwp/api/windows.ui.xaml.controls.appbarseparator)
>- **Las API de biblioteca de la interfaz de usuario de Windows**: [clase CommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout), [clase TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)

Como [CommandBar](app-bars.md), CommandBarFlyout tiene propiedades **PrimaryCommands** y **SecondaryCommands** , que puedes usar para agregar comandos. Puedes colocar los comandos de colección, o ambos. Cuándo y cómo se muestran los comandos principales y secundarios depende del modo de presentación.

El control flotante de barra de comandos tiene dos modos de pantalla: *contraído* y *expandido*.

- En el modo contraído, se muestran solo los comandos principales. Si el control flotante de barra de comandos tiene principales y secundarias comandos, un botón "ver más", que se representa mediante un botón de puntos suspensivos \ [•••\], se muestra. Esto permite al usuario acceder a los comandos secundarios mediante la transición a modo expandido.
- En el modo expandido, se muestran los comandos principales y secundarios. (Si el control tiene solo los elementos secundarios, que son que muestren de forma similar al control MenuFlyout.)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa el control de CommandBarFlyout para mostrar una colección de comandos para el usuario, como los botones y elementos de menú, en el contexto de un elemento en el lienzo de la aplicación.

La TextCommandBarFlyout muestra los comandos de texto en los controles TextBox, RichEditBox, TextBlock, RichTextBlock y PasswordBox. Los comandos se configuran automáticamente correctamente a la selección de texto actual. Usar un CommandBarFlyout para reemplazar los comandos de texto de forma predeterminada en controles de texto.

Para mostrar contextuales comandos en los elementos de lista siguen las instrucciones de [contextuales para colecciones y listas de comandos](collection-commanding.md).

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout vs MenuFlyout

Para mostrar comandos en un menú contextual, puedes usar CommandBarFlyout o MenuFlyout. Te recomendamos CommandBarFlyout porque proporciona más funcionalidad que MenuFlyout. Puedes usar CommandBarFlyout con solo los comandos secundarios para obtener el comportamiento y tener el aspecto de un MenuFlyout o usar el control flotante de barra de comandos completa con los comandos principales y secundarios.

> Para obtener información relacionada, consulta [los controles flotantes](../controls-and-patterns/dialogs-and-flyouts/flyouts.md), [menús y menús contextuales](menus.md)y [las barras de comandos](app-bars.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación de la <strong style="font-weight: semi-bold">Galería de controles XAML</strong> , haz clic aquí para <a href="xamlcontrolsgallery:/item/CommandBarFlyout">Abrir la aplicación y ver el CommandBarFlyout en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>Proactiva frente a invocación reactiva

Por lo general, hay dos formas para invocar un control flotante o un menú que está asociado a un elemento en el lienzo de la interfaz de usuario: _invocación proactiva_ y _reactiva invocación_.

En la invocación proactiva, comandos aparecen automáticamente cuando el usuario interactúa con el elemento que se asocian los comandos. Por ejemplo, los comandos de formato de texto es posible que aparecen cuando el usuario selecciona texto en un cuadro de texto. En este caso, el control flotante de barra de comandos no tiene el foco. En su lugar, presenta relevantes comandos cerca del elemento que el usuario está interactuando con. Si el usuario no interactúa con los comandos, se descarta.

En la invocación reactiva, los comandos se muestran en respuesta a una acción explícita del usuario para solicitar los comandos; Por ejemplo, un ratón. Esto corresponde al concepto tradicional de un [menú contextual](menus.md).

Puedes usar el CommandBarFlyout de manera o incluso una combinación de ambos.

## <a name="create-a-command-bar-flyout"></a>Crear un control flotante de barra de comandos

En este ejemplo se muestra cómo crear un control flotante de barra de comandos y usarla tanto de forma proactiva como reactiva. Cuando se pulsa la imagen, el control flotante se muestra en su modo contraído. Cuando se muestra como un menú contextual, el control flotante se muestra en su modo expandido. En cualquier caso, el usuario puede expandir o contraer el control flotante después de que se abra.

![Ejemplo de un control flotante de barra de comandos contraído](images/command-bar-flyout-img-collapsed.png)

> _Un control flotante de barra de comandos contraído_

![Ejemplo de un control flotante de barra de comandos expandida](images/command-bar-flyout-img-expanded.png)

> _Un control flotante de barra de comandos expandida_

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

### <a name="show-commands-proactively"></a>Mostrar comandos de forma proactiva

Al mostrar comandos contextuales de forma proactiva, solo los comandos principales deben mostrarse de forma predeterminada (el control flotante de barra de comandos debe ser contraído). Coloca los comandos más importantes de la colección de comandos principales y los comandos adicionales que tradicionalmente colocaba en un menú contextual en la colección de comandos secundarios.

Para mostrar de forma proactiva los comandos, normalmente se controla el evento [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) o [Tapped](/uwp/api/windows.ui.xaml.uielement.tapped) para mostrar el control flotante de barra de comandos. Establece el control del flotante [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) **transitorios** o **TransientWithDismissOnPointerMoveAway** para abrir el control flotante en su modo contraído sin toma el foco.

A partir de Windows 10 Insider Preview, controles de texto tienen una propiedad de **SelectionFlyout** . Cuando un control flotante se asigna a esta propiedad, se muestra automáticamente cuando se selecciona texto.

### <a name="show-commands-reactively"></a>Mostrar comandos reactiva

Al mostrar comandos contextuales reactiva, como un menú contextual, los comandos secundarios se muestran de forma predeterminada (el control flotante de barra de comandos debe ser expandido). En este caso, el control flotante de barra de comandos puede tener los comandos principales y secundarios, o solo hay comandos secundarios.

Para mostrar comandos en un menú contextual, por lo general, se asigna el control flotante a la propiedad [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) de un elemento de interfaz de usuario. De esta forma, abrir el control flotante se controla mediante el elemento y no tienes que hacer nada más.

Si controlas mostrando el control flotante (por ejemplo, en un evento [RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped) ), Establece el control del flotante [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) en **estándar** para abrir el control flotante en su modo expandido y asignarle el foco.

> [!TIP]
> Para obtener más información acerca de las opciones al mostrar un control flotante y cómo controlar la posición del control flotante, consulta [los controles flotantes](../controls-and-patterns/dialogs-and-flyouts/flyouts.md).

## <a name="commands-and-content"></a>Comandos y contenido

El control de CommandBarFlyout tiene 2 propiedades que puedes usar para agregar comandos y contenido: [PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands) y [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands).

De forma predeterminada, los elementos de las barras de comandos se agregan a la colección **PrimaryCommands**. Estos comandos se muestran en la barra de comandos y son visibles en los modos contraídos y expandidos. A diferencia de CommandBar, los comandos principales no lo desbordamiento para los comandos secundarios automáticamente y se truncarán.

También puedes agregar comandos a la colección **SecondaryCommands** . Los comandos secundarios se muestran en la parte de menú del control y están visibles solo en el modo expandido.

### <a name="app-bar-buttons"></a>Botones de la barra de la aplicación

Puedes rellenar las propiedades PrimaryCommands y SecondaryCommands directamente con los controles [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx)y [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) .

Los controles de botón de la barra de la aplicación se caracterizan por un icono y una etiqueta de texto. Estos controles están optimizados para usarse en una barra de comandos y su aspecto cambia en función de si el control se muestra en la barra de comandos o el menú de desbordamiento.

- Botones de barra de la aplicación usados como comandos principales se muestran en la barra de comandos con solo su icono; no se muestra la etiqueta de texto. Te recomendamos que uses una información sobre herramientas para mostrar una descripción de texto del comando, como se muestra aquí.
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- Botones de barra de la aplicación usados como comandos secundarios se muestran en el menú, con la etiqueta y el icono visible.

### <a name="other-content"></a>Otro contenido

Puedes agregar otros controles a un control flotante de barra de comandos por encapsularlos en un AppBarElementContainer. Esto te permite agregar controles como [DropDownButton]() o el [botón de división]()o agregar contenedores como [StackPanel]() para crear la interfaz de usuario más compleja.

Con el fin de agregarse a las colecciones de comando principal o secundario de un control flotante de barra de comandos, un elemento debe implementar la interfaz de [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement) . AppBarElementContainer es un contenedor que implementa esta interfaz para que pueda agregar un elemento a una barra de comandos incluso si no implementa la interfaz de sí mismo.

Aquí, un AppBarElementContainer se usa para agregar elementos adicionales a un control flotante de barra de comandos. Un botón de división se agrega a los comandos principales para permitir la selección de colores. Un elemento StackPanel se agrega a los comandos secundarios para permitir que un diseño más complejo para controles de zoom.

> [!TIP]
> De manera predeterminada, elementos diseñados para el lienzo de la aplicación podrían no tener el aspecto adecuados en una barra de comandos. Cuando agregas un elemento mediante AppBarElementContainer, hay algunos pasos que debe seguir para hacer que el elemento que coincida con otros elementos de la barra de comandos:
>
> - Reemplazar los pinceles de forma predeterminada con el [estilo ligero](/design/controls-and-patterns/xaml-styles#lightweight-styling) para hacer que el elemento en segundo plano y border coinciden con los botones de barra de la aplicación.
> - Ajustar el tamaño y la posición del elemento.
> - Ajustar los iconos en un cuadro de vídeo con un ancho y alto de 16 píxeles.

> [!NOTE]
> En este ejemplo se muestra solo el comando barra control flotante de interfaz de usuario, no implementa cualquiera de los comandos que se muestran. Para obtener más información acerca de cómo implementar los comandos, consulta [los botones](buttons.md) y los [conceptos básicos del diseño de comandos](../basics/commanding-basics.md).

![Un control flotante de barra de comandos con un botón en dos paneles](images/command-bar-flyout-split-button.png)

> _Un control flotante de barra de comandos contraído con un botón de división abierto_

![Un control flotante de barra de comandos con la interfaz de usuario compleja](images/command-bar-flyout-custom-ui.png)

> _Un control flotante de barra de comandos expandida con la interfaz de usuario de zoom personalizado en el menú_


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

## <a name="create-a-context-menu-with-secondary-commands-only"></a>Crear un menú contextual con los comandos secundarios solo

Puedes usar un CommandBarFlyout con solo los comandos secundarios como un [menú contextual](menus.md), en lugar de un MenuFlyout.

![Un control flotante de barra de comandos con solo los comandos secundarios](images/command-bar-flyout-context-menu.png)

> _Control flotante de barra de comandos como un menú contextual_

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

También puedes usar un CommandBarFlyout con un DropDownButton para crear un menú estándar.

![Un control de barra de comandos flotante con como un botón menú desplegable](images/command-bar-flyout-dropdown.png)

> _Un botón de menú en un control flotante de barra de comandos desplegable_

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

## <a name="command-bar-flyouts-for-text-controls"></a>Controles flotantes de la barra de comandos para los controles de texto

El [TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) es un control flotante de barra de comandos especializados que contiene comandos de edición de texto. Cada control de texto muestra el TextCommandBarFlyout automáticamente como un menú contextual (botón secundario), o cuando hay texto seleccionado. El control flotante la barra de comandos de texto se adapta a la selección de texto para mostrar solo los comandos pertinentes.

![Un control flotante la barra de comandos de texto contraído](images/command-bar-flyout-text-selection.png)

> _Un control flotante de barra de comandos texto en la selección de texto_

![Un control flotante la barra de comandos de texto expandido](images/command-bar-flyout-text-full.png)

> _Un control flotante la barra de comandos de texto expandido_


### <a name="available-commands"></a>Comandos disponibles

Esta tabla muestra los comandos que se incluyen en un TextCommandBarFlyout y que se muestren.

| Comando | Se muestra … |
| ------- | -------- |
| Negrita | Cuando el control de texto no es de solo lectura (RichEditBox solo). |
| Cursiva | Cuando el control de texto no es de solo lectura (RichEditBox solo). |
| Subrayado | Cuando el control de texto no es de solo lectura (RichEditBox solo). |
| Corrección | Cuando IsSpellCheckEnabled es **true** y escrita de texto está seleccionado. |
| Cortar | Cuando el control de texto no es de solo lectura y hay texto seleccionado. |
| Copiar | Cuando se selecciona el texto. |
| Pegar | Cuando el control de texto no es de solo lectura y el Portapapeles tiene contenido. |
| Deshacer | Cuando se produce una acción que se puede deshacer. |
| Seleccionar todos | Cuando se puede seleccionar texto. |

### <a name="custom-text-command-bar-flyouts"></a>Controles flotantes de barra de comandos de texto personalizado

TextCommandBarFlyout no se puede personalizar y se administra automáticamente por cada control de texto. Sin embargo, puedes reemplazar el valor predeterminado TextCommandBarFlyout con comandos personalizados.

- Para reemplazar el valor predeterminado TextCommandBarFlyout que se muestra en la selección de texto, puedes crear un CommandBarFlyout personalizado (u otro tipo de control flotante) y asignarlo a la propiedad **SelectionFlyout** . Si estableces SelectionFlyout en **null**, no hay comandos se muestran en la selección.
- Para reemplazar el valor predeterminado TextCommandBarFlyout que se muestra como el menú contextual, asignar un CommandBarFlyout personalizado (u otro tipo de control flotante) a la propiedad **ContextFlyout** en un control de texto. Si estableces ContextFlyout en **null**, se muestra el menú flotante que se muestra en las versiones anteriores del control de texto en lugar de la TextCommandBarFlyout.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.
- [Muestra de comandos XAML](http://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>Artículos relacionados

- [Conceptos básicos de diseño de los comandos para las aplicaciones para UWP](../basics/commanding-basics.md)
- [Clase CommandBar](https://msdn.microsoft.com/library/windows/apps/dn279427)
