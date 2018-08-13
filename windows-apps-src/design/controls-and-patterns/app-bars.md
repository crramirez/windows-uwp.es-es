---
author: serenaz
Description: Command bars give users easy access to your app's most common tasks.
title: Barra de comandos
label: App bars/command bars
template: detail.hbs
op-migration-status: ready
ms.author: sezhen
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 868b4145-319b-4a97-82bd-c98d966144db
pm-contact: yulikl
design-contact: ksulliv
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d8a55643a598a90805f45b614cadd4db64d4c8a3
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
ms.locfileid: "1691554"
---
# <a name="command-bar"></a>Barra de comandos

Las barras de comandos proporcionan a los usuarios acceso fácil a las tareas más comunes de tu aplicación. Las barras de comandos pueden proporcionar acceso a los comandos del nivel de aplicaciones o específicos de la página y se pueden usar con cualquier patrón de navegación.

> **API importantes**: [Clase CommandBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx), [Clase AppBarButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarbutton.aspx), [Clase AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbartogglebutton.aspx), [Clase AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarseparator.aspx)

![Ejemplo de una barra de comandos con iconos](images/controls_appbar_icons.png)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

El control CommandBar es un control de propósito general, flexible y ligero que puede mostrar contenido complejo, como imágenes o bloques de texto, así como comandos simples, como por ejemplo, controles [AppBarButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbartogglebutton.aspx) y [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarseparator.aspx).

> [!NOTE]
XAML proporciona tanto el control [AppBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar) como el control [CommandBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar). El control AppBar solo se debería usar cuando se actualiza una aplicación universal para Windows8 que usa dicho control y se necesita minimizar los cambios. Para nuevas aplicaciones de Windows 10, es recomendable usar el control CommandBar en su lugar. Para este documento, se supone que usas el control CommandBar.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/CommandBar">abrir la aplicación y ver CommandBar en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Una barra de comandos expandida en la aplicación Fotos de Microsoft.

![Barra de comandos de la aplicación Fotos de Microsoft](images/control-examples/command-bar-photos.png)

Una barra de comandos en el Calendario de Outlook en Windows Phone.

![Barra de comandos de la aplicación Calendario de Outlook](images/control-examples/command-bar-calendar-phone.png)

## <a name="anatomy"></a>Anatomía

De manera predeterminada, en la barra de comandos se muestra una fila de botones de icono y un botón "ver más" opcional, que se representa mediante puntos suspensivos \[•••\]. Esta es la barra de comandos creada mediante el código de ejemplo que se muestra más adelante. Se muestra en su estado cerrado y compacto.

![Barra de comandos cerrada](images/command-bar-compact.png)

La barra de comandos también se puede mostrar en un estado cerrado y mínimo, con este aspecto. Para obtener más información, consulta la sección [Estados abiertos y cerrados](#open-and-closed-states).

![Barra de comandos cerrada](images/command-bar-minimal.png)

Esta es la misma barra de comandos, en su estado abierto. Las etiquetas identifican las partes principales del control.

![Barra de comandos cerrada](images/commandbar_anatomy_open.png)

La barra de comandos se divide en 4 áreas principales:
- El área de contenido se alinea a la izquierda de la barra. Se muestra si la propiedad [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx) está completada.
- El área de comandos principales se alinea a la derecha de la barra. Se muestra si la propiedad [PrimaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx) está completada.  
- El botón "ver más" \[•••\] se muestra en la parte derecha de la barra. Presionar el botón "ver más" \[•••\] revela las etiquetas de los comandos principales y abre el menú de desbordamiento si hay comandos secundarios. El botón no estará visible si no hay etiquetas de comandos principales o etiquetas secundarias presentes. Para cambiar el comportamiento predeterminado, usa la propiedad [OverflowButtonVisibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.overflowbuttonvisibility.aspx).
- El menú de desbordamiento solo se muestra cuando la barra de comandos está abierta y la propiedad [SecondaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx) está completada. Cuando el espacio sea limitado, los comandos principales se trasladarán al área SecondaryCommands. Para cambiar el comportamiento predeterminado, usa la propiedad [IsDynamicOverflowEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.isdynamicoverflowenabled.aspx).

El diseño se invierte cuando la propiedad [FlowDirection](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.flowdirection.aspx) es **RightToLeft**.

## <a name="create-a-command-bar"></a>Crear una barra de comandos
En este ejemplo, se crea la barra de comandos mostrada anteriormente.

```xaml
<CommandBar>
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle" Click="AppBarButton_Click" />
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat" Click="AppBarButton_Click"/>
    <AppBarSeparator/>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Forward" Label="Forward" Click="AppBarButton_Click"/>

    <CommandBar.SecondaryCommands>
        <AppBarButton Label="Like" Click="AppBarButton_Click"/>
        <AppBarButton Label="Dislike" Click="AppBarButton_Click"/>
    </CommandBar.SecondaryCommands>

    <CommandBar.Content>
        <TextBlock Text="Now playing..." Margin="12,14"/>
    </CommandBar.Content>
</CommandBar>
```

## <a name="commands-and-content"></a>Comandos y contenido
El control CommandBar tiene 3 propiedades que puedes usar para agregar comandos y contenido: [PrimaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx), [SecondaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx) y [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx).


### <a name="commands"></a>Comandos

De forma predeterminada, los elementos de las barras de comandos se agregan a la colección **PrimaryCommands**. Debes agregar los comandos por orden de importancia, para que los comandos más importantes estén siempre visibles. Cuando cambia el ancho de la barra de comandos, como cuando los usuarios cambian el tamaño de la ventana de la aplicación, los comandos principales se mueven dinámicamente entre la barra de comandos y el menú de desbordamiento por puntos de interrupción. Para cambiar este comportamiento predeterminado, usa la propiedad [IsDynamicOverflowEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.isdynamicoverflowenabled.aspx). 

En las pantallas más pequeñas (320 epx de ancho), caben 4 comandos principales como máximo en la barra de comandos. 

También puedes agregar comandos a la colección **SecondaryCommands**, que se muestran en el menú de desbordamiento.

![Ejemplo de una barra de comandos con un área e iconos "Más"](images/appbar_rs2_overflow_icons.png)

Puedes mover los comandos mediante programación entre los objetos PrimaryCommands y SecondaryCommands, según sea necesario.

- *Si un comando aparece de manera coherente entre las páginas, es mejor mantener ese comando en una ubicación coherente.*
- *Se recomienda colocar los comandos Aceptar y Sí a la izquierda de los comandos Rechazar, No y Cancelar. La coherencia le da confianza al usuario para moverse por el sistema y le ayuda a transferir su conocimiento sobre la navegación de una aplicación a otra.*

### <a name="app-bar-buttons"></a>Botones de la barra de la aplicación

Las propiedades PrimaryCommands y SecondaryCommands solo se pueden rellenar con elementos de comando [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx) y [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx). 

Los controles de botón de la barra de la aplicación se caracterizan por un icono y una etiqueta de texto. Estos controles están optimizados para usarse en una barra de comandos y su aspecto cambia en función de si se usa el control en la barra de comandos o el menú de desbordamiento.

El tamaño de los iconos del menú de desbordamiento es de 16 x 16 píxeles, que es menor que los iconos del área de comandos principales (con un tamaño de 20 x 20 píxeles). Si usas SymbolIcon, FontIcon o PathIcon, el icono ajusta automáticamente en el tamaño correcto, sin pérdida de fidelidad cuando el comando entra en el área de comandos secundaria. 

### <a name="button-labels"></a>Etiquetas de los botones
La propiedad AppBarButton [IsCompact](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton.IsCompact) determina si se muestra la etiqueta. En un control CommandBar, la barra de comandos sobrescribe la propiedad IsCompact del botón automáticamente cuando la barra se abre y se cierra.

Para colocar las etiquetas de los botones de barra de la aplicación, usa la propiedad [DefaultLabelPosition](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.defaultlabelposition.aspx) de CommandBar.

```xaml
<CommandBar DefaultLabelPosition="Right">
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle"/>
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat"/>
</CommandBar>
```

![Barra de comandos con etiquetas a la derecha](images/app-bar-labels-on-right.png)

En las ventanas más grandes, plantéate mover las etiquetas a la derecha de los iconos de botón de la barra de la aplicación para mejorar la legibilidad. Las etiquetas de la parte inferior requieren que los usuarios abran la barra de comandos para revelar las etiquetas, mientras que las etiquetas a la derecha son visibles incluso cuando se cierra la barra de comandos.

En los menús de desbordamiento, las etiquetas están a la derecha de los iconos de manera predeterminada y se ignora **LabelPosition**. Puedes ajustar el estilo estableciendo la propiedad [CommandBarOverflowPresenterStyle](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar.CommandBarOverflowPresenterStyle) en un Style destinado al objeto [CommandBarOverflowPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbaroverflowpresenter). 

Las etiquetas de los botones deben ser cortas, preferiblemente una única palabra. Las etiquetas más largas que hay debajo de un icono se ajustarán en varias líneas, lo que aumentará el alto total de la barra de comandos abierta. Puedes incluir un carácter de guion suave (0x00AD) en el texto de una etiqueta para sugerir en el límite de carácter dónde se debe producir un salto de palabra. En XAML, esto se expresa esta mediante una secuencia de escape, tal como se muestra abajo:

```xaml
<AppBarButton Icon="Back" Label="Areally&#x00AD;longlabel"/>
```

Cuando la etiqueta se ajusta en la ubicación sugerida, tiene este aspecto.

![Botón de la barra de la aplicación con etiqueta de ajuste](images/app-bar-button-label-wrap.png)

### <a name="command-bar-flyouts"></a>Controles flotantes de la barra de comandos

Considera la posibilidad de realizar agrupaciones lógicas de los comandos, como colocar Responder, Responder a todos y Reenviar en un menú Responder. Aunque generalmente un botón de la barra de la aplicación activa un solo comando, se lo puede usar para mostrar un [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.menuflyout.aspx) o un [Control flotante](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flyout.aspx) con contenido personalizado.

![Ejemplo de controles flotantes en una barra de comandos](images/AppbarGuidelines_Flyouts.png)

### <a name="other-content"></a>Otro contenido

Puedes agregar cualquier elemento XAML al área de contenido al establecer la propiedad **Content**. Si quieres agregar más de un elemento, tendrías que colocarlos en un contenedor de panel y hacer que el panel sea un elemento secundario único de la propiedad Content.

Cuando se habilite un desbordamiento dinámico, el contenido no se recortará porque los comandos principales pueden desplazarse al menú de desbordamiento. De lo contrario, los comandos principales reciben prioridad y pueden provocar que el contenido se recorte.

Cuando la propiedad [ClosedDisplayMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closeddisplaymode.aspx) es **Compact**, el contenido se puede recortar si es mayor que el tamaño compacto de la barra de comandos. Debes controlar los eventos [Opening](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opening.aspx) y [Closed](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closed.aspx) para mostrar u ocultar partes de la interfaz de usuario en el área de contenido de modo que no se recorten. Para obtener más información, consulta la sección [Estados abiertos y cerrados](#open-and-closed-states).


## <a name="open-and-closed-states"></a>Estados abiertos y cerrados

La barra de comandos se puede abrir o cerrar. Cuando está abierta, se muestran los botones de comando principales con etiquetas de texto y el menú de desbordamiento se abre si hay comandos secundarios presentes.

Un usuario puede alternar entre ambos estados al presionar el botón "ver más" \[•••\]. Puedes cambiar entre ellos mediante programación al establecer la propiedad [IsOpen](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.isopen.aspx). 

Puedes usar los eventos [Opening](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opening.aspx), [Opened](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opened.aspx), [Closing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closing.aspx) y [Closed](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closed.aspx) para responder a la apertura o cierre de la barra de comandos.  
- Los eventos Opened y Closed se producen antes del inicio de la animación de transición.
- Los eventos Opened y Closed se producen una vez finalizada la transición.

En este ejemplo, los eventos Opening y Closing se usan para cambiar la opacidad de la barra de comandos. Cuando se cierra la barra de comandos, se hace de forma semitransparente para que el fondo de la aplicación se muestre. Cuando se abre la barra de comandos, la barra de comandos se hace opaca para que el usuario pueda centrarse en los comandos.

```xaml
<CommandBar Opening="CommandBar_Opening"
            Closing="CommandBar_Closing">
    <AppBarButton Icon="Accept" Label="Accept"/>
    <AppBarButton Icon="Edit" Label="Edit"/>
    <AppBarButton Icon="Save" Label="Save"/>
    <AppBarButton Icon="Cancel" Label="Cancel"/>
</CommandBar>
```

```csharp
private void CommandBar_Opening(object sender, object e)
{
    CommandBar cb = sender as CommandBar;
    if (cb != null) cb.Background.Opacity = 1.0;
}

private void CommandBar_Closing(object sender, object e)
{
    CommandBar cb = sender as CommandBar;
    if (cb != null) cb.Background.Opacity = 0.5;
}

```

### <a name="issticky"></a>IsSticky

Si un usuario interactúa con otras partes de una aplicación cuando una barra de comandos está abierta, se cerrará la barra automáticamente. Esto se denomina *cierre del elemento por cambio de foco*. Puedes controlar el comportamiento del cierre del elemento por cambio de foco ajustando configurando la propiedad [IsSticky](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.issticky.aspx). Cuando `IsSticky="true"`, la barra permanece abierta hasta que el usuario presiona el botón "ver más" \[•••\] o selecciona un elemento del menú de desbordamiento. 

Recomendamos evitar usar las barras de comando permanentes porque no cumplen con las expectativas de los usuarios sobre el cierre del elemento por cambio de foco.

### <a name="display-mode"></a>Modo de pantalla

Puedes controlar cómo se muestra la barra de comandos en su estado cerrado al establecer la propiedad [ClosedDisplayMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closeddisplaymode.aspx). Hay tres modos de visualización de estado cerrado entre los que puedes elegir:
- **Compacta**: El modo predeterminado. Se muestra el contenido, los iconos de comando principales sin etiquetas y el botón "ver más" \[•••].
- **Mínima**: Se muestra solo una barra delgada que actúa como el botón "ver más" \[•••]. El usuario puede presionar en cualquier parte de la barra para abrirla.
- **Oculta**: Cuando se cierra, la barra de comandos no se muestra. Esto puede ser de utilidad para mostrar comandos contextuales con una barra de comandos alineada. En este caso, tienes que abrir la barra de comandos mediante programación estableciendo la propiedad **IsOpen** o cambiando la propiedad ClosedDisplayMode a **Mínima** o **Compacta**.

Aquí, se usa una barra de comandos para contener comandos de formato simples para un objeto [RichEditBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx). Cuando el cuadro de edición no tiene el foco, los comandos de formato pueden resultar una distracción, por lo que se ocultan. Cuando se usa el cuadro de edición, la propiedad ClosedDisplayMode de la barra de comandos cambia a Compacta, de modo que se muestren los comandos de formato.

```xaml
<StackPanel Width="300"
            GotFocus="EditStackPanel_GotFocus"
            LostFocus="EditStackPanel_LostFocus">
    <CommandBar x:Name="FormattingCommandBar" ClosedDisplayMode="Hidden">
        <AppBarButton Icon="Bold" Label="Bold" ToolTipService.ToolTip="Bold"/>
        <AppBarButton Icon="Italic" Label="Italic" ToolTipService.ToolTip="Italic"/>
        <AppBarButton Icon="Underline" Label="Underline" ToolTipService.ToolTip="Underline"/>
    </CommandBar>
    <RichEditBox Height="200"/>
</StackPanel>
```

```csharp
private void EditStackPanel_GotFocus(object sender, RoutedEventArgs e)
{
    FormattingCommandBar.ClosedDisplayMode = AppBarClosedDisplayMode.Compact;
}

private void EditStackPanel_LostFocus(object sender, RoutedEventArgs e)
{
    FormattingCommandBar.ClosedDisplayMode = AppBarClosedDisplayMode.Hidden;
}
```

>**Nota**&nbsp;&nbsp;La implementación de los comandos de edición está fuera del ámbito de este ejemplo. Para más información, consulta el artículo sobre la propiedad [RichEditBox](rich-edit-box.md).

Aunque los modos Mínima y Oculta son de utilidad en algunas situaciones, ten en cuenta que ocultar todas las acciones podría confundir a los usuarios.

Cambiar la propiedad ClosedDisplayMode para proporcionar más o menos información de una sugerencia al usuario afecta al diseño de los elementos adyacentes. Por el contrario, cuando el objeto CommandBar cambia entre cerrado y abierto, no afecta al diseño de otros elementos.

## <a name="placement"></a>Colocación
Las barras de comandos se pueden colocar en la parte superior de la ventana de la aplicación, en la parte inferior de la ventana de la aplicación y de manera alineada.

![Ejemplo 1 de colocación de barra de la aplicación](images/AppbarGuidelines_Placement1.png)

-   Para dispositivos portátiles pequeños, te recomendamos ubicar las barras de comandos en la parte inferior de la pantalla para conseguir una mayor disponibilidad.
-   En los dispositivos con pantallas más grandes, al colocar las barras de comandos cerca de la parte superior de la ventana, estas destacan más y son reconocibles.

Usa la API [DiagonalSizeInInches](https://msdn.microsoft.com/library/windows/apps/windows.graphics.display.displayinformation.diagonalsizeininches.aspx) para determinar el tamaño físico de la pantalla.

Las barras de comandos pueden colocarse en las siguientes regiones de la pantalla en pantallas de vista única (ejemplo de la izquierda) y en las pantallas de varias vistas (ejemplo de la derecha). Las barras de comandos alineadas pueden colocarse en cualquier lugar del espacio de acción.

![Ejemplo 2 de colocación de barra de la aplicación](images/AppbarGuidelines_Placement2.png)

>**Dispositivos táctiles**: Si la barra de comandos debe permanecer visible para el usuario cuando aparece el teclado táctil o el panel de entrada de software (SIP), puedes asignarla a la propiedad [BottomAppBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.bottomappbar.aspx) de una página y pasará a estar visible cuando el SIP esté presente. De lo contrario, debes colocar la barra de comandos de manera alineada y en una posición relativa al contenido de tu aplicación.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.
- [Muestra de comandos XAML](http://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>Artículos relacionados

* [Conceptos básicos de diseño de los comandos para las aplicaciones para UWP](../basics/commanding-basics.md)
* [Clase CommandBar](https://msdn.microsoft.com/library/windows/apps/dn279427)