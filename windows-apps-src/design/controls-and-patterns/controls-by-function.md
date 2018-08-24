---
author: Jwmsft
Description: Provides a list by function of some of the controls that you can use in your apps.
title: Controles por función
ms.assetid: 8DB4347B-91D6-4659-91F2-80ECF7BBB596
label: Controls by function
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0840bab2e039ec55ea4070f8dad39c0ae4e74bbc
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "2836828"
---
# <a name="controls-by-function"></a>Controles por función

El marco de trabajo de la interfaz de usuario de XAML para Windows proporciona una biblioteca amplia de controles compatibles con el desarrollo de la interfaz de usuario. Algunos de estos controles tienen una representación visual y otros funcionan como contenedores de otros controles o de contenido, como imágenes y multimedia. 

Puedes ver muchos de los controles de interfaz de usuario de Windows si descargas la [Muestra de conceptos básicos de la interfaz de usuario XAML](http://go.microsoft.com/fwlink/p/?LinkId=619992).

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tiene la aplicación de la <strong style="font-weight: semi-bold">Galería de controles de XAML</strong> instalada, haga clic aquí para <a href="xamlcontrolsgallery:/item/NavigationView">Abrir la aplicación y vea la NavigationView en acción</a> </p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>


Aquí se muestra una lista por función de los controles de XAML comunes que puedes usar en tu aplicación.

## <a name="appbars-and-commands"></a>Barras de la aplicación y comandos

### <a name="app-bar"></a>Barra de aplicaciones
Barra de herramientas para mostrar comandos específicos de la aplicación. Consulta Barra de comandos.

Referencia: [AppBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.aspx) 

### <a name="app-bar-button"></a>Botón de la barra de la aplicación
Botón para mostrar comandos con estilo de barra de la aplicación.

![Iconos del botón de la barra de la aplicación](images/controls/app-bar-buttons.png) 

Referencia: [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx), [SymbolIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.symbolicon.aspx), [BitmapIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.bitmapicon.aspx), [FontIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.fonticon.aspx), [PathIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pathicon.aspx) 

Diseño y procedimientos: [Guía de control de la barra de la aplicación y la barra de comandos](app-bars.md) 

Código de muestra: [Muestra de comandos de XAML](http://go.microsoft.com/fwlink/p/?LinkId=620019)

### <a name="app-bar-separator"></a>Separador de la barra de la aplicación
Separa visualmente grupos de comandos en una barra de comandos.

Referencia: [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) 

Código de muestra: [Muestra de comandos de XAML](http://go.microsoft.com/fwlink/p/?LinkId=620019)

### <a name="app-bar-toggle-button"></a>Botón de alternancia de la barra de la aplicación
Un botón para alternar comandos en una barra de comandos.

Referencia: [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx) 

Código de muestra: [Muestra de comandos de XAML](http://go.microsoft.com/fwlink/p/?LinkId=620019)

### <a name="command-bar"></a>Barra de comandos
Barra de la aplicación especial que gestiona el cambio de tamaño de los elementos de botón de la barra de la aplicación.

![Control de barra de comandos](images/command-bar-compact.png)

```xaml
<CommandBar>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
</CommandBar>
```
Referencia: [CommandBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.aspx) 

Diseño y procedimientos: [Guía de control de la barra de la aplicación y la barra de comandos](app-bars.md)

Código de muestra: [Muestra de comandos de XAML](http://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="buttons"></a>Botones

### <a name="button"></a>Botón
Control que responde a la entrada del usuario y que genera un evento **Click**.

![Un botón estándar](images/controls/button.png)

```xaml
<Button x:Name="button1" Content="Button" 
        Click="Button_Click" />
```

Referencia: [botón](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.button.aspx) 

Diseño y procedimientos: [Guía de control de botones](buttons.md) 

### <a name="hyperlink"></a>Hipervínculo
Consulta el botón de hipervínculo.

### <a name="hyperlink-button"></a>Botón de hipervínculo
Botón que aparece como texto marcado y abre el URI especificado en un explorador.

![Botón de hipervínculo](images/controls/hyperlink-button.png)

```xaml
<HyperlinkButton Content="www.microsoft.com" 
                 NavigateUri="http://www.microsoft.com"/>
```

Referencia: [HyperlinkButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.hyperlinkbutton.aspx) 

Diseño y procedimientos: [Guía de control de hipervínculos](hyperlinks.md)

### <a name="repeat-button"></a>Botón Repetir
Botón que genera su evento **Click** repetidamente desde que se presiona hasta que se suelta. 

![Un control de botón Repetir](images/controls/repeat-button.png) 

```xaml
<RepeatButton x:Name="repeatButton1" Content="Repeat Button" 
              Click="RepeatButton_Click" />
```

Referencia: [RepeatButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.repeatbutton.aspx) 

Diseño y procedimientos: [Guía de control de botones](buttons.md) 

## <a name="collectiondata-controls"></a>Controles de datos o colección

### <a name="flip-view"></a>Invertir vista
Control que presenta una colección de elementos por los que el usuario se puede desplazar rápidamente, de uno en uno.

```xaml
<FlipView x:Name="flipView1" SelectionChanged="FlipView_SelectionChanged">
    <Image Source="Assets/Logo.png" />
    <Image Source="Assets/SplashScreen.png" />
    <Image Source="Assets/SmallLogo.png" />
</FlipView>
```

Referencia: [FlipView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flipview.aspx) 

Diseño y procedimientos: [Guía de control de invertir vista](flipview.md) 

### <a name="grid-view"></a>Vista de cuadrícula
Control que presenta una colección de elementos en filas y columnas por las que es posible desplazarse verticalmente.

```xaml
<GridView x:Name="gridView1" SelectionChanged="GridView_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
</GridView>
```

Referencia: [GridView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridview.aspx) 

Diseño y procedimientos: [Listas](lists.md) 

Código de muestra: [Muestra de ListView](http://go.microsoft.com/fwlink/p/?LinkId=619900)

### <a name="items-control"></a>Control de elementos
Control que presenta una colección de elementos en una interfaz de usuario especificada por una plantilla de datos. 

```xaml
<ItemsControl/>
```

Referencia: [ItemsControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx) 

### <a name="list-view"></a>Vista de lista
Control que presenta una colección de elementos en una lista por la que podemos desplazarnos horizontalmente.

```xaml
<ListView x:Name="listView1" SelectionChanged="ListView_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
</ListView>
```

Referencia: [ListView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listview.aspx) 

Diseño y procedimientos: [Listas](lists.md) 

Código de muestra: [Muestra de ListView](http://go.microsoft.com/fwlink/p/?LinkId=619900)

## <a name="date-and-time-controls"></a>Controles de fecha y hora

### <a name="calendar-date-picker"></a>Selector de fecha del calendario
Control que permite a un usuario seleccionar una fecha mediante una presentación de calendario desplegable.

![Un selector de fecha del calendario con vista de calendario abierta](images/controls/calendar-date-picker-open.png)

```xaml
<CalendarDatePicker/>
```

Referencia: [CalendarDatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.aspx) 

Diseño y procedimientos: [Controles de calendario, fecha y hora](date-and-time.md)
 
### <a name="calendar-view"></a>Vista de calendario
Pantalla de calendario configurable que permite a un usuario seleccionar una o varias fechas.

```xaml
<CalendarView/>
```

Referencia: [CalendarView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.aspx) 

Diseño y procedimientos: [Controles de calendario, fecha y hora](date-and-time.md) 

### <a name="date-picker"></a>Selector de fecha
Control que permite a un usuario seleccionar una fecha.

![Control de selector de fecha](images/controls/date-picker.png)

```xaml
<DatePicker Header="Arrival Date"/>
```

Referencia: [DatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx) 

Diseño y procedimientos: [Controles de calendario, fecha y hora](date-and-time.md)
 
### <a name="time-picker"></a>Selector de hora
Control que permite a un usuario seleccionar un valor de hora.

![Control TimePicker](images/controls/time-picker.png) 

```xaml
<TimePicker Header="Arrival Time"/>
```

Referencia: [TimePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx) 

Diseño y procedimientos: [Controles de calendario, fecha y hora](date-and-time.md)

## <a name="flyouts"></a>Controles flotantes

### <a name="context-menu"></a>Menú contextual
Consulta Menú flotante y Menú emergente.

### <a name="flyout"></a>Control flotante
Muestra un mensaje que requiere la intervención del usuario. (Al contrario que los cuadros de diálogo, los controles flotantes no crean una ventana independiente ni bloquean otra interacción del usuario).

![Control de control flotante](images/controls/flyout.png)

```xaml
<Flyout>
    <StackPanel>
        <TextBlock Text="All items will be removed. Do you want to continue?"/>
        <Button Click="DeleteConfirmation_Click" Content="Yes, empty my cart"/>
    </StackPanel>
</Flyout>
```

Referencia: [Control flotante](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flyout.aspx) 

Diseño y procedimientos: [menús emergentes](dialogs-and-flyouts/flyouts.md) 

### <a name="menu-flyout"></a>Control flotante de menú
Muestra de forma temporal una lista de comandos u opciones relacionados con lo que está haciendo el usuario.

![Control de control flotante de menú](images/controls/menu-flyout.png) 

```xaml
<MenuFlyout>
    <MenuFlyoutItem Text="Reset" Click="Reset_Click"/>
    <MenuFlyoutSeparator/>
    <ToggleMenuFlyoutItem Text="Shuffle" 
                          IsChecked="{Binding IsShuffleEnabled, Mode=TwoWay}"/>
    <ToggleMenuFlyoutItem Text="Repeat" 
                          IsChecked="{Binding IsRepeatEnabled, Mode=TwoWay}"/>
</MenuFlyout>
```

Referencia: [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyout.aspx), [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx), [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx), [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) 

Diseño y procedimientos: [menús y menús contextuales](menus.md) 

Código de muestra: [Muestra del menú contextual XAML](http://go.microsoft.com/fwlink/p/?LinkId=620021)

### <a name="popup-menu"></a>Menú emergente
Menú personalizado que presenta los comandos que especifiques.

Referencia: [PopupMenu](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.popups.popupmenu.aspx) 

Diseño y procedimientos: [cuadros de diálogo](dialogs-and-flyouts/dialogs.md) 

### <a name="tooltip"></a>Información sobre herramientas
Ventana emergente que muestra información para un elemento. 
 
![Control de información sobre herramientas](images/controls/tool-tip.png)

```xaml
<Button Content="Button" Click="Button_Click" 
        ToolTipService.ToolTip="Click to perform action" />
```

Referencia: [ToolTip](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.tooltip.aspx), [ToolTipService](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.tooltipservice.aspx) 

Diseño y procedimientos: directrices para obtener información sobre herramientas 

## <a name="images"></a>Imágenes

### <a name="image"></a>Imagen
Control que presenta una imagen.

```xaml
<Image Source="Assets/Logo.png" />
```

Referencia: [Imagen](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx) 

Diseño y procedimientos: [Imagen e ImageBrush](images-imagebrushes.md) 

Código de muestra: [Muestra de imágenes XAML](http://go.microsoft.com/fwlink/p/?linkid=226867)

## <a name="graphics-and-ink"></a>Gráficos y entrada de lápiz

### <a name="inkcanvas"></a>InkCanvas
Control que recibe y muestra trazos de lápiz.

```xaml
<InkCanvas/>
```

Referencia: [InkCanvas](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.inkcanvas.aspx) 

### <a name="shapes"></a>Formas
Diversos objetos gráficos en modo retenido que se pueden representar como elipses, rectángulos, líneas, trazados Bézier, etc.

![Un polígono](images/controls/shapes-polygon.png) 
![Una ruta de acceso](images/controls/shapes-path.png) 

```xaml
<Ellipse/>
<Path/>
<Rectangle/>
```

Referencia: [Formas](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.aspx) 

Procedimientos: [Dibujar formas](../../graphics/drawing-shapes.md) 

Código de muestra: [Muestra de dibujo basado en vectores de XAML](http://go.microsoft.com/fwlink/p/?linkid=226866)

## <a name="layout-controls"></a>Controles de diseño

### <a name="border"></a>Borde
Control de contenedor que dibuja un borde, fondo o ambos alrededor de otro objeto.

![Un borde alrededor de 2 rectángulos](images/controls/border.png) 

```xaml
<Border BorderBrush="Blue" BorderThickness="4" 
        Height="108" Width="64" 
        Padding="8" CornerRadius="4">
    <Canvas>
        <Rectangle Fill="Orange"/>
        <Rectangle Fill="Green" Margin="0,44"/>
    </Canvas>
</Border>
```

Referencia: [Borde](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.border.aspx)

### <a name="canvas"></a>Lienzo
Panel de diseño que admite el posicionamiento absoluto de elementos secundarios relativos a la esquina superior izquierda del lienzo.
 
![Panel de diseño de Canvas](images/controls/canvas.png) 

```xaml
<Canvas Width="120" Height="120">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Canvas.Left="20" Canvas.Top="20"/>
    <Rectangle Fill="Green" Canvas.Left="40" Canvas.Top="40"/>
    <Rectangle Fill="Orange" Canvas.Left="60" Canvas.Top="60"/>
</Canvas>
```

Referencia: [Lienzo](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx)
 
### <a name="grid"></a>Cuadrícula
Un panel de diseño que permite reorganizar los elementos secundarios en filas y columnas.

![Panel de diseño de cuadrícula](images/controls/grid.png) 

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="50"/>
        <RowDefinition Height="50"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="50"/>
        <ColumnDefinition Width="50"/>
    </Grid.ColumnDefinitions>
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Grid.Row="1"/>
    <Rectangle Fill="Green" Grid.Column="1"/>
    <Rectangle Fill="Orange" Grid.Row="1" Grid.Column="1"/>
</Grid>
```

Referencia: [Cuadrícula](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx)
 
### <a name="panning-scroll-viewer"></a>Visor de desplazamiento panorámico
Consulta Visor de desplazamiento.

### <a name="relativepanel"></a>RelativePanel
Panel que te permite colocar y alinear objetos secundarios relacionados entre sí o el panel primario.

![Panel de diseño RelativePanel](images/controls/relative-panel.png) 

```xaml
<RelativePanel>
    <TextBox x:Name="textBox1" RelativePanel.AlignLeftWithPanel="True"/>
    <Button Content="Submit" RelativePanel.Below="textBox1"/>
</RelativePanel>
```

Referencia: [RelativePanel](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx)

### <a name="scroll-bar"></a>Barra de desplazamiento
Consulta Visor de desplazamiento. (ScrollBar es un elemento de ScrollViewer. Generalmente, no lo usas como un control independiente).

Referencia: [ScrollBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.scrollbar.aspx)
 
### <a name="scroll-viewer"></a>Visor de desplazamiento
Un control de contenedor que permite al usuario ver vistas panorámicas y acercar el contenido.

```xaml
<ScrollViewer ZoomMode="Enabled" MaxZoomFactor="10" 
              HorizontalScrollMode="Enabled" 
              HorizontalScrollBarVisibility="Visible"
              Height="200" Width="200">
    <Image Source="Assets/Logo.png" Height="400" Width="400"/>
</ScrollViewer>
```

Referencia: [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.aspx)

Diseño y procedimientos: [Guía de control de movimiento panorámico y desplazamiento](scroll-controls.md) 

Código de muestra: [Muestra de desplazamiento, movimiento panorámico y zoom XAML](http://go.microsoft.com/fwlink/p/?linkid=238577)

### <a name="stack-panel"></a>Panel de pila
Panel de diseño en el que los elementos secundarios se organizan en una sola línea y se pueden orientar horizontal o verticalmente.

![Control de diseño de panel de pila](images/controls/stack-panel.png) 

```xaml
<StackPanel>
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue"/>
    <Rectangle Fill="Green"/>
    <Rectangle Fill="Orange"/>
</StackPanel>
```

Referencia: [StackPanel](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx)
 
### <a name="variablesizedwrapgrid"></a>VariableSizedWrapGrid
Un panel de diseño que permite reorganizar los elementos secundarios en filas y columnas. Cada elemento secundario puede abarcar varias filas y columnas.

![Panel de diseño de cuadrícula de ajuste de tamaño variable](images/controls/variable-sized-wrap-grid.png) 

```xaml
<VariableSizedWrapGrid MaximumRowsOrColumns="3" ItemHeight="44" ItemWidth="44">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Height="80" 
               VariableSizedWrapGrid.RowSpan="2"/>
    <Rectangle Fill="Green" Width="80" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
    <Rectangle Fill="Orange" Height="80" Width="80" 
               VariableSizedWrapGrid.RowSpan="2" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
</VariableSizedWrapGrid>
```

Referencia: [VariableSizedWrapGrid](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.aspx)

### <a name="viewbox"></a>Cuadro de vídeo
Control de contenedor que escala su contenido a un tamaño especificado.

![Control de cuadro de vídeo](images/controls/view-box.png) 

```xaml
<Viewbox MaxWidth="25" MaxHeight="25">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
<Viewbox MaxWidth="75" MaxHeight="75">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
<Viewbox MaxWidth="150" MaxHeight="150">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
```

Referencia: [Viewbox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.viewbox.aspx)
 
### <a name="zooming-scroll-viewer"></a>Visor de desplazamiento de acercamiento
Consulta Visor de desplazamiento.

## <a name="media-controls"></a>Controles multimedia

### <a name="audio"></a>Audio
Consulta Elemento multimedia

### <a name="media-element"></a>Elemento multimedia
Control que reproduce contenido de audio y vídeo.

```xaml
<MediaElement x:Name="myMediaElement"/>
```

Referencia: [MediaElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediaelement.aspx) 

Diseño y procedimientos: [Guía de control de elemento multimedia](media-playback.md)

### <a name="mediatransportcontrols"></a>MediaTransportControls
Control que proporciona controles de reproducción para un MediaElement.

![Elemento multimedia con controles de transporte](images/controls/media-transport-controls.png) 

```xaml
<MediaTransportControls MediaElement="myMediaElement"/>
```

Referencia: [MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.aspx) 

Diseño y procedimientos: [Guía de control de elemento multimedia](media-playback.md) 

Código de muestra: [Muestra de controles de transporte de medios](http://go.microsoft.com/fwlink/p/?LinkId=620023)

### <a name="video"></a>Vídeo
Consulta Elemento multimedia

## <a name="navigation"></a>Navegación

### <a name="navigationview"></a>NavigationView

Un contenedor adaptable y el modelo de navegación flexible que implementa el panel de navegación izquierdo, la navegación superior y el patrón de fichas.

Referencia: [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)

Diseño y procedimientos: [Guía de control de NavigationView](navigationview.md)

### <a name="splitview"></a>SplitView

Control de contenedor con dos vistas; una vista para el contenido principal y otra vista que se usa normalmente para un menú de navegación.

![Control de vista en dos paneles](images/controls/split-view.png) 

```xaml
<SplitView>
    <SplitView.Pane>
        <!-- Menu content -->
    </SplitView.Pane>
    <SplitView.Content>
        <!-- Main content -->
    </SplitView.Content>
</SplitView>
```

Referencia: [SplitView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.splitview.aspx) 

Diseño y procedimientos: [Guía de control de vista en dos paneles](split-view.md)

### <a name="web-view"></a>Vista web

Control de contenedor que hospeda el contenido web.

```xaml
<WebView x:Name="webView1" Source="http://dev.windows.com" 
         Height="400" Width="800"/>
```

Referencia: [WebView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.aspx) 

Diseño y procedimientos: Directrices para vistas web 

Código de muestra: [Muestra de control WebView de XAML](http://go.microsoft.com/fwlink/p/?linkid=238582)

### <a name="semantic-zoom"></a>Zoom semántico

Control de contenedor que permite al usuario acercarse entre dos vistas de una colección de elementos.

```xaml
<SemanticZoom>
    <ZoomedInView>
        <GridView></GridView>
    </ZoomedInView>
    <ZoomedOutView>
        <GridView></GridView>
    </ZoomedOutView>
</SemanticZoom>
```

Referencia: [SemanticZoom](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.aspx) 

Diseño y procedimientos: [Guía de control de zoom semántico](semantic-zoom.md)

Código de muestra: [Muestra de agrupación de GridView y SemanticZoom XAML](http://go.microsoft.com/fwlink/p/?linkid=226564)

## <a name="progress-controls"></a>Controles de progreso

### <a name="progress-bar"></a>Barra de progreso
Control que indica el progreso mediante una barra.

![Control de barra de progreso](images/controls/progress-bar-determinate.png)

Barra de progreso que muestra un valor específico.

```xaml
<ProgressBar x:Name="progressBar1" Value="50" Width="100"/>
```

![Control de barra de progreso indeterminado](images/controls/progress-bar-indeterminate.png)

Barra de progreso que muestra un progreso indeterminado.

```xaml
<ProgressBar x:Name="indeterminateProgressBar1" IsIndeterminate="True" Width="100"/>
```

Referencia: [ProgressBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.aspx) 

Diseño y procedimientos: [Guía de control del progreso](progress-controls.md) 

### <a name="progress-ring"></a>Círculo de progreso
Control que indica el progreso indeterminado mediante un círculo. 

![Control de círculo de progreso](images/controls/progress-ring.png) 

```xaml
<ProgressRing x:Name="progressRing1" IsActive="True"/>
```

Referencia: [ProgressRing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.aspx) 

Diseño y procedimientos: [Guía de control del progreso](progress-controls.md) 

## <a name="text-controls"></a>Controles de texto

### <a name="auto-suggest-box"></a>Cuadro de sugerencias automáticas
Un cuadro de entrada de texto que proporciona texto sugerido a medida que el usuario escribe.

![Un cuadro de sugerencias automáticas para búsqueda](images/controls/auto-suggest-box.png) 

Referencia: [AutoSuggestBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.aspx)

Diseño y procedimientos: [Controles de texto](text-controls.md), [Guía de control del cuadro de sugerencia automática](auto-suggest-box.md)

Código de muestra: [Muestra de migración de AutoSuggestBox](http://go.microsoft.com/fwlink/p/?LinkId=619996)

### <a name="multi-line-text-box"></a>Cuadro de texto multilínea
Consulta Cuadro de texto.

### <a name="password-box"></a>Cuadro de contraseña
Control para escribir contraseñas.

 ![Un cuadro de contraseña](images/controls/password-box.png)

```xaml
<PasswordBox x:Name="passwordBox1" 
             PasswordChanged="PasswordBox_PasswordChanged" />
```

Referencia: [PasswordBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.aspx) 

Diseño y procedimientos: [Controles de texto](text-controls.md), [Guía de control de cuadro de contraseña](password-box.md) 

Código de muestra: [Muestra de visualización de texto XAML](http://go.microsoft.com/fwlink/p/?linkid=238579), [Muestra de edición de texto XAML](http://go.microsoft.com/fwlink/p/?linkid=251417)

### <a name="rich-edit-box"></a>Cuadro de texto enriquecido
Control que permite a un usuario editar documentos de texto enriquecido con contenido como texto con formato, hipervínculos e imágenes.

```xaml
<RichEditBox />
```

Referencia: [RichEditBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx) 

Diseño y procedimientos: [Controles de texto](text-controls.md), [Guía de control del cuadro de edición con formato](rich-edit-box.md)

Código de muestra: [Muestra de texto XAML](http://go.microsoft.com/fwlink/p/?linkid=238578)

### <a name="search-box"></a>Cuadro de búsqueda
Consulta Cuadro de sugerencias automáticas.

### <a name="single-line-text-box"></a>Cuadro de texto de una línea
Consulta Cuadro de texto.

### <a name="static-textparagraph"></a>Texto/Párrafo estático
Consulta Bloque de texto.

### <a name="text-block"></a>Bloque de texto
Control que muestra texto.

![Control de bloque de texto](images/controls/text-block.png) 

```xaml
<TextBlock x:Name="textBlock1" Text="I am a TextBlock"/>
```

Referencia: [TextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx), [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx) 

Diseño y procedimiento: [Controles de texto](text-controls.md), [Guía de control de bloque de texto](text-block.md), [Guía de control de bloque de texto enriquecido](rich-text-block.md)

Código de muestra: [Muestra de texto XAML](http://go.microsoft.com/fwlink/p/?linkid=238578)

### <a name="text-box"></a>Cuadro de texto
Campo de texto sin formato de una línea o multilínea.

![Control de cuadro de texto](images/controls/text-box.png) 

```xaml
<TextBox x:Name="textBox1" Text="I am a TextBox" 
         TextChanged="TextBox_TextChanged"/>
```

Referencia: [TextBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx) 

Diseño y procedimientos: [Controles de texto](text-controls.md), [Guía de control de cuadro de texto](text-box.md) 

Código de muestra: [Muestra de texto XAML](http://go.microsoft.com/fwlink/p/?linkid=238578)

## <a name="selection-controls"></a>Controles de selección

### <a name="check-box"></a>Casilla
Control que un usuario puede activar y desactivar.

![Los tres estados de una casilla](images/templates-checkbox-states-default.png)

```xaml
<CheckBox x:Name="checkbox1" Content="CheckBox" 
          Checked="CheckBox_Checked"/>
```

Referencia: [CheckBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.checkbox.aspx) 

Diseño y procedimientos: [Guía de control de casillas](checkbox.md) 

### <a name="combo-box"></a>Cuadro combinado
Lista desplegable de elementos entre los que puede seleccionar un usuario.

![Cuadro combinado abierto](images/controls/combo-box-open.png) 

```xaml
<ComboBox x:Name="comboBox1" Width="100"
          SelectionChanged="ComboBox_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
</ComboBox>
```

Referencia: [ComboBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.combobox.aspx) 

Diseño y procedimientos: [Listas](lists.md) 

### <a name="list-box"></a>Cuadro de lista
Control que presenta una lista en línea de elementos entre los que puede seleccionar un usuario. 

![Control de cuadro de lista](images/controls/list-box.png)

```xaml
<ListBox x:Name="listBox1" Width="100"
         SelectionChanged="ListBox_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
</ListBox>
```

Referencia: [ListBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listbox.aspx) 

Diseño y procedimientos: [Listas](lists.md) 

### <a name="radio-button"></a>Botón de radio
Control que permite que el usuario seleccione una sola opción entre un grupo de ellas. Cuando se agrupan los botones de radio, se excluyen mutuamente.

![Controles de botón de radio](images/controls/radio-button.png)

```xaml
<RadioButton x:Name="radioButton1" Content="RadioButton 1" GroupName="Group1" 
             Checked="RadioButton_Checked"/>
<RadioButton x:Name="radioButton2" Content="RadioButton 2" GroupName="Group1" 
             Checked="RadioButton_Checked" IsChecked="True"/>
<RadioButton x:Name="radioButton3" Content="RadioButton 3" GroupName="Group1" 
             Checked="RadioButton_Checked"/>
```

Referencia: [RadioButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.radiobutton.aspx) 

Diseño y procedimientos: [Guía de control del botón de radio](radio-button.md)
 
### <a name="slider"></a>Control deslizante
Control que permite que el usuario seleccione entre un intervalo de valores moviendo un control Thumb por una pista.

![Control de control deslizante](images/controls/slider.png)

```xaml
<Slider x:Name="slider1" Width="100" ValueChanged="Slider_ValueChanged" />
```

Referencia: [Control deslizante](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.slider.aspx) 

Diseño y procedimientos: [Guía de control de control deslizante](slider.md) 

### <a name="toggle-button"></a>Botón de alternancia
Botón que se puede alternar entre dos estados.

```xaml
<ToggleButton x:Name="toggleButton1" Content="Button" 
              Checked="ToggleButton_Checked"/>
```

Referencia: [ToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.togglebutton.aspx)

Diseño y procedimientos: [Guía de control de alternancia](toggles.md) 

### <a name="toggle-switch"></a>Modificador para alternar
Modificador que se puede alternar entre dos estados.

![Control de modificador para alternar](images/controls/toggle-switch.png) 

```xaml
<ToggleSwitch x:Name="toggleSwitch1" Header="ToggleSwitch" 
              OnContent="On" OffContent="Off" 
              Toggled="ToggleSwitch_Toggled"/>
```

Referencia: [ToggleSwitch](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.toggleswitch.aspx) 

Diseño y procedimientos: [Guía de control de alternancia](toggles.md) 
