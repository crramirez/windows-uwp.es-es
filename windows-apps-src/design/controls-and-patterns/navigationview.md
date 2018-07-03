---
author: serenaz
Description: Control that provides top-level app navigation with an automatically adapting, collapsible left navigation menu
title: Vista de navegación
ms.assetid: ''
label: Navigation view
template: detail.hbs
ms.author: sezhen
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: vasriram
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c7817bf7ff60a52ea48c988bdebd6d4d2eeacdb7
ms.sourcegitcommit: 618741673a26bd718962d4b8f859e632879f9d61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "1992154"
---
# <a name="navigation-view"></a>Vista de navegación

El control de vista de navegación proporciona un menú de navegación contraíble para la navegación de nivel superior de tu aplicación. Este control implementa el panel de navegación, o menú hamburguesa, y adapta automáticamente el modo de presentación del panel a diferentes tamaños de ventana.

> **API importantes**: [Clase NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview), [Clase NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem), [Enumeración NavigationViewDisplayMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewdisplaymode)

![Ejemplo de NavigationView](images/navview_wireframe.png)

## <a name="video-summary"></a>Resumen en vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev010/player]

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

NavigationView es adecuado para:

-  Muchos elementos de navegación de nivel superior de un tipo similar. (Por ejemplo, una aplicación de deportes con categorías como "fútbol", "béisbol", "baloncesto", "fútbol americano", etc.)
-  Un número de medio a alto (5-10) de categorías de navegación de nivel superior.
-  Proporcionar una experiencia de navegación coherente. El panel de navegación debe incluir solo los elementos de navegación, no las acciones.
-  Conservación de la superficie de pantalla de ventanas menores.

NavigationView es solo uno de varios elementos de navegación que puedes usar. Para obtener más información sobre otros elementos y patrones de navegación, consulta [Conceptos básicos del diseño de navegación](../basics/navigation-basics.md).

El control de NavigationView tiene muchos comportamientos integrados que implementan el patrón de panel de navegación sencilla. Si la navegación requiere un comportamiento más complejo que no es compatible con NavigationView, quizá te interese el patrón [Panel maestro y detalles](master-details.md) en su lugar.

## <a name="examples"></a>Ejemplos
<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/NavigationView">abrir la aplicación y ver NavigationView en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="navigationview-sections"></a>Secciones de NavigationView

![Secciones de NavigationView](images/navview_sections.png)

### <a name="pane"></a>Panel

El botón de navegación integrado ("hamburguesa") permite a los usuarios abrir y cerrar el panel. En ventanas de la aplicación mayores, cuando el panel está abierto, puedes ocultar este botón usando la propiedad [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible). La etiqueta de texto junto a la hamburguesa es la propiedad [PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle).

El botón Atrás integrado aparece en la esquina superior izquierda en el panel. El control NavigationView no agrega contenido automáticamente a la pila de retroceso, pero para habilitar la navegación hacia atrás, consulta la sección [navegación hacia atrás](#backwards-navigation).

El panel NavigationView también puede contener:

- Elementos de navegación, en forma de [NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem), para navegar a páginas específicas
- Separadores, en forma de [NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator), para agrupar elementos de navegación
- Encabezados, en forma de [NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader), para etiquetar grupos de elementos
- Un control [AutoSuggestBox](auto-suggest-box.md) opcional para permitir la búsqueda de nivel de aplicación
- Un punto de entrada opcional para la [configuración de la aplicación](../app-settings/app-settings-and-data.md). Para ocultar el elemento de configuración, usa la propiedad [IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible)
- Contenido de forma libre en el pie de página del panel, cuando se agrega a la propiedad [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter)

#### <a name="visual-style"></a>Estilo visual

Los elementos de NavigationView admiten estados visuales seleccionados, deshabilitados, con el puntero encima, presionados y enfocados.

![Estados de elementos NavigationView: deshabilitado, con el puntero encima, presionado, enfocado](images/navview_item-states.png)

Cuando se cumplen los requisitos de hardware y software, NavigationView usa automáticamente el nuevo [Material acrílico](../style/acrylic.md) y [Mostrar resaltado](../style/reveal.md) en su panel.

### <a name="header"></a>Encabezado

El área de encabezado se alinea verticalmente con el botón de navegación y tiene una altura fija de 52 píxeles. Su finalidad es contener el título de la página de la categoría de navegación seleccionada. El encabezado se acopla a la parte superior de la página y actúa como punto de recorte de desplazamiento para el área de contenido.

El encabezado debe estar visible cuando NavigationView se encuentra en el modo mínimo. Puedes elegir ocultar el encabezado de otros modos, que se usan en los anchos de ventana mayores. Para ello, establece la propiedad [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) en **false**.

### <a name="content"></a>Contenido

El área de contenido es donde se muestra la mayor parte de la información para la categoría de navegación seleccionada. 

Te recomendamos márgenes de 12 píxeles en el área de tu contenido cuando NavigationView se encuentre en el modo mínimo y márgenes de 24 píxeles en caso contrario.

## <a name="navigationview-display-modes"></a>Modos de pantalla NavigationView
El panel NavigationView puede abrirse o cerrarse, y tiene tres opciones de modo de pantalla:
-  **Mínimo** Solo el botón hamburguesa permanece fijo mientras que se muestra y se oculta el panel según sea necesario.
-  **Compacto** El panel siempre se muestra como una franja estrecha que se puede abrir hasta el ancho completo.
-  **Expandido** El panel permanece abierto a lo largo del contenido. Cuando se cierra activando el botón hamburguesa, el ancho del panel pasa a ser una franja estrecha.

De manera predeterminada, el sistema selecciona automáticamente el modo óptimo de visualización según la cantidad de espacio de pantalla disponible para el control. (Puedes [deshabilitar](#overriding-the-default-adaptive-behavior) esta configuración.)

### <a name="minimal"></a>Mínimo

![NavigationView en el modo mínimo, que muestra el panel abierto y cerrado](images/navview_minimal.png)

-  Cuando se cierra, el panel se oculta de manera predeterminada, solo con el botón de navegación visible.
-  Ofrece una navegación a petición que ahorra superficie en pantalla. Ideal para aplicaciones en teléfonos y tabléfonos.
-  El panel se abre y se cierra presionando el botón de navegación, que se dibuja como una superposición sobre el encabezado y el contenido. El contenido no se redistribuye.
-  Cuando esté abierto, el panel es transitorio y se puede cerrar con un gesto de cierre del elemento por cambio de foco, como realizar una selección, presionar el botón Atrás o pulsar fuera del panel.
-  El elemento seleccionado se vuelve visible cuando se abre la superposición del panel.
-  Cuando se cumplen los requisitos, el fondo del panel abierto es [acrílico desde la aplicación](../style/acrylic.md#acrylic-blend-types).
-  De manera predeterminada, NavigationView se encuentra en el modo mínimo cuando su ancho total es menor o igual que 640 píxeles.

### <a name="compact"></a>Compacto

![NavigationView en el modo compacto, que muestra el panel abierto y cerrado](images/navview_compact.png)

-  Cuando se cierra, son visibles una franja vertical del panel que muestra solo iconos y el botón de navegación.
-  Proporciona alguna indicación de la ubicación seleccionada, mientras usa una pequeña cantidad de la superficie de la pantalla.
-  Este modo es más adecuado para pantallas medianas, como tabletas y [experiencias de 10 pies](../devices/designing-for-tv.md).
-  El panel se abre y se cierra presionando el botón de navegación, que se dibuja como una superposición sobre el encabezado y el contenido. El contenido no se redistribuye.
-  El encabezado no es necesario y se puede ocultar para ofrecer al contenido un espacio más vertical.
-  El elemento seleccionado muestra un indicador visual para resaltar el lugar en el que se encuentra el usuario en el árbol de navegación.
-  Cuando se cumplen los requisitos, el fondo del panel es [acrílico desde la aplicación](../style/acrylic.md#acrylic-blend-types).
-  De manera predeterminada, NavigationView está en modo compacto cuando su ancho total se encuentra entre 641 y 1007 píxeles.

### <a name="expanded"></a>Expandido

![NavigationView en modo expandido, que muestra el panel abierto](images/navview_expanded.png)

-  De forma predeterminada, el panel permanece abierto. Este modo es más adecuado para pantallas más grandes.
-  El panel se dibuja en paralelo con el encabezado y el contenido, que se redistribuye dentro de su espacio disponible.
-  Cuando el panel se cierra con el botón de navegación, el panel aparece como una franja estrecha, en paralelo con el encabezado y el contenido.
-  El encabezado no es necesario y se puede ocultar para ofrecer al contenido un espacio más vertical.
-  El elemento seleccionado muestra un indicador visual para resaltar el lugar en el que se encuentra el usuario en el árbol de navegación.
-  Cuando se cumplen los requisitos, el fondo del panel se pinta con [acrílico en segundo plano](../style/acrylic.md#acrylic-blend-types).
-  De manera predeterminada, NavigationView está en modo expandido cuando su ancho total es superior a 1007 píxeles.

### <a name="overriding-the-default-adaptive-behavior"></a>Invalidación del comportamiento adaptable predeterminado

NavigationView cambia automáticamente su modo de pantalla en función de la cantidad de espacio en pantalla disponible para ella.

> [!NOTE]
> NavigationView debe servir como el contenedor raíz de la aplicación, ya que este control está diseñado para ocupar todo el ancho y el alto de la ventana de la aplicación.
Puedes invalidar los anchos en los que la vista de navegación cambia los modos de visualización usando las propiedades [CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.CompactModeThresholdWidth) y [ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ExpandedModeThresholdWidth).

Ten en cuenta los siguientes escenarios que muestran cuándo es posible que quieras personalizar el comportamiento del modo de pantalla.

- **Navegación frecuente** Si esperas que los usuarios naveguen entre las áreas de la aplicación un poco con frecuencia, piensa en la posibilidad de mantener el panel en la vista en anchos de ventana menores. Una aplicación de música con áreas de navegación de canciones/álbumes/artistas puede optar por un ancho de panel de 280 píxeles y permanecer en el modo expandido cuando el ancho de la ventana de la aplicación sea superior a 560 píxeles.
```xaml
<NavigationView OpenPaneLength="280" CompactModeThresholdWidth="560" ExpandedModeThresholdWidth="560"/>
```

- **Navegación poco frecuente** Si esperas que los usuarios naveguen entre las áreas de aplicación con poca frecuencia, plantéate mantener el panel oculto para anchos de ventana mayores. Una aplicación de calculadora con varios diseños puede optar por permanecer en el modo mínimo incluso cuando la aplicación se maximiza en una pantalla de 1080 píxeles.
```xaml
<NavigationView CompactModeThresholdWidth="1920" ExpandedModeThresholdWidth="1920"/>
```

- **Desambiguación de iconos** Si las áreas de navegación de la aplicación no se prestan por sí mismas para iconos significativos, evita usar el modo compacto. Una aplicación de visualización de imágenes con colecciones/álbumes/carpetas puede optar por mostrar NavigationView en el modo mínimo en anchos estrechos y medianos y en el modo expandido en el ancho amplio.
```xaml
<NavigationView CompactModeThresholdWidth="1008"/>
```

## <a name="interaction"></a>Interacción

Cuando los usuarios pulsen en un elemento de navegación en el panel, NavigationView mostrará ese elemento como seleccionado y generará un evento [ItemInvoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked). Si la pulsación da por resultado la selección de un nuevo elemento, NavigationView también generará un evento [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged). 

Tu aplicación es responsable de actualizar el encabezado y el contenido con la información adecuada en respuesta a la interacción de este usuario. Además, te recomendamos mover el [foco](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control.FocusState) mediante programación desde el elemento de navegación hasta el contenido. Al establecer el foco inicial en la carga, optimizas el flujo del usuario y minimizas el número previsto de movimientos del foco del teclado.

## <a name="backwards-navigation"></a>Navegación hacia atrás
NavigationView tiene un botón Atrás integrado, que se puede habilitar con las siguientes propiedades:
- [**IsBackButtonVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible) es una enumeración NavigationViewBackButtonVisible y "Auto" de manera predeterminada. Se utiliza para mostrar u ocultar el botón Atrás. Cuando el botón no está visible, se contraerá el espacio para dibujar el botón Atrás.
- [**IsBackEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled) es falso de manera predeterminada y puede usarse para alternar los estados del botón Atrás.
- [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) se activa cuando un usuario hace clic en el botón Atrás.
    - En el modo mínimo/compacto, cuando NavigationView.Pane está abierto como un control flotante, al hacer clic en el botón Atrás se cerrará el panel y se activará en su lugar el evento **PaneClosing**.
    - No se activará si IsBackEnabled es falso.

![Botón Atrás de NavigationView](../basics/images/back-nav/NavView.png)

## <a name="code-example"></a>Ejemplo de código

El siguiente es un ejemplo sencillo de cómo puedes incorporar NavigationView en tu aplicación. 

Aquí se muestra cómo implementar la navegación hacia atrás con el botón Atrás de NavigationView. Ten en cuenta que para usar las propiedades de la navegación hacia atrás de NavigationView, necesitarás [Windows 10 Insider Preview (introducida en v10.0.17110.0)](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK).

Aquí también se muestra la localización de las cadenas de elementos de contenido de navegación con `x:Uid`. Para obtener más información sobre la localización, consulta [Localizar cadenas en tu interfaz de usuario](../../app-resources/localize-strings-ui-manifest.md).

```xaml
<Page
    x:Class="NavigationViewSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:NavigationViewSample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <NavigationView x:Name="NavView"
                    ItemInvoked="NavView_ItemInvoked"
                    Loaded="NavView_Loaded"
                    BackRequested="NavView_BackRequested">

        <NavigationView.MenuItems>
            <NavigationViewItem x:Uid="HomeNavItem" Content="Home" Tag="home">
                <NavigationViewItem.Icon>
                    <FontIcon Glyph="&#xE10F;"/>
                </NavigationViewItem.Icon>
            </NavigationViewItem>
            <NavigationViewItemSeparator/>
            <NavigationViewItemHeader Content="Main pages"/>
            <NavigationViewItem x:Uid="AppsNavItem" Icon="AllApps" Content="Apps" Tag="apps"/>
            <NavigationViewItem x:Uid="GamesNavItem" Icon="Video" Content="Games" Tag="games"/>
            <NavigationViewItem x:Uid="MusicNavItem" Icon="Audio" Content="Music" Tag="music"/>
        </NavigationView.MenuItems>

        <NavigationView.AutoSuggestBox>
            <AutoSuggestBox x:Name="ASB" QueryIcon="Find"/>
        </NavigationView.AutoSuggestBox>

        <NavigationView.HeaderTemplate>
            <DataTemplate>
                <Grid Margin="24,10,0,0">
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto"/>
                        <ColumnDefinition/>
                    </Grid.ColumnDefinitions>
                    <TextBlock Style="{StaticResource TitleTextBlockStyle}"
                           FontSize="28"
                           VerticalAlignment="Center"
                           Text="Welcome"/>
                    <CommandBar Grid.Column="1"
                            HorizontalAlignment="Right"
                            VerticalAlignment="Top"
                            DefaultLabelPosition="Right"
                            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
                        <AppBarButton Label="Refresh" Icon="Refresh"/>
                        <AppBarButton Label="Import" Icon="Import"/>
                    </CommandBar>
                </Grid>
            </DataTemplate>
        </NavigationView.HeaderTemplate>

        <NavigationView.PaneFooter>
            <HyperlinkButton x:Name="MoreInfoBtn"
                             Content="More info"
                             Click="More_Click"
                             Margin="12,0"/>
        </NavigationView.PaneFooter>

        <Frame x:Name="ContentFrame" Margin="24">
            <Frame.ContentTransitions>
                <TransitionCollection>
                    <NavigationThemeTransition/>
                </TransitionCollection>
            </Frame.ContentTransitions>
        </Frame>

    </NavigationView>
</Page>
```

```csharp
private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    // you can also add items in code behind
    NavView.MenuItems.Add(new NavigationViewItemSeparator());
    NavView.MenuItems.Add(new NavigationViewItem()
    { Content = "My content", Icon = new SymbolIcon(Symbol.Folder), Tag = "content" });

    // set the initial SelectedItem 
    foreach (NavigationViewItemBase item in NavView.MenuItems)
    {
        if (item is NavigationViewItem && item.Tag.ToString() == "home")
        {
            NavView.SelectedItem = item;
            break;
        }
    }
            
    ContentFrame.Navigated += On_Navigated;

    // add keyboard accelerators for backwards navigation
    KeyboardAccelerator GoBack = new KeyboardAccelerator();
    GoBack.Key = VirtualKey.GoBack;
    GoBack.Invoked += BackInvoked;
    KeyboardAccelerator AltLeft = new KeyboardAccelerator();
    AltLeft.Key = VirtualKey.Left;
    AltLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(GoBack);
    this.KeyboardAccelerators.Add(AltLeft);
    // ALT routes here
    AltLeft.Modifiers = VirtualKeyModifiers.Menu;
    
}

private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{  
    if (args.IsSettingsInvoked)
    {
        ContentFrame.Navigate(typeof(SettingsPage));
    }
    else
    {
        // find NavigationViewItem with Content that equals InvokedItem
        var item = sender.MenuItems.OfType<NavigationViewItem>().First(x => (string)x.Content == (string)args.InvokedItem);
        NavView_Navigate(item as NavigationViewItem);
    }
}

private void NavView_Navigate(NavigationViewItem item)
{
    switch (item.Tag)
    {
        case "home":
            ContentFrame.Navigate(typeof(HomePage));
            break;

        case "apps":
            ContentFrame.Navigate(typeof(AppsPage));
            break;

        case "games":
            ContentFrame.Navigate(typeof(GamesPage));
            break;

        case "music":
            ContentFrame.Navigate(typeof(MusicPage));
            break;

        case "content":
            ContentFrame.Navigate(typeof(MyContentPage));
            break;
    }           
}

private void NavView_BackRequested(NavigationView sender, NavigationViewBackRequestedEventArgs args)
{
    On_BackRequested();
}

private void BackInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}

private bool On_BackRequested()
{
    bool navigated = false;

    // don't go back if the nav pane is overlayed
    if (NavView.IsPaneOpen && (NavView.DisplayMode == NavigationViewDisplayMode.Compact || NavView.DisplayMode == NavigationViewDisplayMode.Minimal))
    {
        return false;
    }
    else
    {
        if (ContentFrame.CanGoBack)
        {
            ContentFrame.GoBack();
            navigated = true;
        }
    }
    return navigated;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        NavView.SelectedItem = NavView.SettingsItem as NavigationViewItem;
    }
    else 
    {
        Dictionary<Type, string> lookup = new Dictionary<Type, string>()
        {
            {typeof(HomePage), "home"},
            {typeof(AppsPage), "apps"},
            {typeof(GamesPage), "games"},
            {typeof(MusicPage), "music"},
            {typeof(MyContentPage), "content"}    
        };

        String stringTag = lookup[ContentFrame.SourcePageType];

        // set the new SelectedItem  
        foreach (NavigationViewItemBase item in NavView.MenuItems)
        {
            if (item is NavigationViewItem && item.Tag.Equals(stringTag))
            {
                item.IsSelected = true;
                break;
            }
        }        
    }
}
```

## <a name="customizing-backgrounds"></a>Personalización de fondos

Para cambiar el fondo del área principal de NavigationView, establece su propiedad `Background` en tu pincel preferido.

El fondo del panel muestra acrílico desde la aplicación cuando NavigationView está en el modo Mínimo o Compacto, y acrílico en el fondo en el modo Expandido. Para actualizar este comportamiento o personalizar la apariencia del acrílico del panel, modifica los dos recursos de tema sobrescribiéndolos en tu App.xaml.

```xaml
<Application.Resources>
    <ResourceDictionary>
        <AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
        BackgroundSource="Backdrop" TintColor="Yellow" TintOpacity=".6"/>
        <AcrylicBrush x:Key="NavigationViewExpandedPaneBackground"
        BackgroundSource="HostBackdrop" TintColor="Orange" TintOpacity=".8"/>
    </ResourceDictionary>
</Application.Resources>
```

## <a name="extending-your-app-into-the-title-bar"></a>Ampliar la aplicación a la barra de título

Para obtener un aspecto unificado y fluido dentro de la ventana de la aplicación, se recomienda ampliar NavigationView y su panel acrílico al área de la barra de título de la aplicación. Esto evita la forma poco atractiva visualmente que crea la barra de título, el contenido de NavigationView de color sólido y el acrílico del panel de NavigationView.

Para ello, agrega el siguiente código a App.xaml.cs.

```csharp
//draw into the title bar
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
coreTitleBar.ExtendViewIntoTitleBar = true;

//remove the solid-colored backgrounds behind the caption controls and system back button
var viewTitleBar = ApplicationView.GetForCurrentView().TitleBar;
viewTitleBar.ButtonBackgroundColor = Colors.Transparent;
viewTitleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
viewTitleBar.ButtonForegroundColor = (Color)Resources["SystemBaseHighColor"];
```

Dibujar en la barra de título tiene el efecto secundario de ocultar el título de la aplicación. Para ayudar a los usuarios, restaura el título añadiendo tu propio TextBlock. Agrega el siguiente marcado a la página raíz que contiene tu NavigationView.

```xaml
<Grid>
    <TextBlock x:Name="AppTitle"
        xmlns:appmodel="using:Windows.ApplicationModel"
        Text="{x:Bind appmodel:Package.Current.DisplayName}"
        Style="{StaticResource CaptionTextBlockStyle}"
        IsHitTestVisible="False"
        Canvas.ZIndex="1"/>
    

    <NavigationView Canvas.ZIndex="0" ... />

</Grid>
```

También deberás ajustar los márgenes del AppTitle según la visibilidad del botón Atrás. Y cuando la aplicación está en FullScreenMode, deberás quitar el espacio de la flecha atrás, aunque la barra de título le reserve espacio.

```csharp
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
Window.Current.SetTitleBar(AppTitle);
coreTitleBar.ExtendViewIntoTitleBar = true;

void UpdateAppTitle()
{
    var full = (ApplicationView.GetForCurrentView().IsFullScreenMode);
    var left = 12 + (full ? 0 : CoreApplication.GetCurrentView().TitleBar.SystemOverlayLeftInset);
    AppTitle.Margin = new Thickness(left, 8, 0, 0);
}

Window.Current.CoreWindow.SizeChanged += (s, e) => UpdateAppTitle();
coreTitleBar.LayoutMetricsChanged += (s, e) => UpdateAppTitle();
```

Para obtener más información acerca de cómo personalizar barras de título, consulta [personalización de la barra de título](../shell/title-bar.md).

## <a name="related-topics"></a>Artículos relacionados

- [Clase NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [Maestro/detalles](master-details.md)
- [Control dinámico](tabs-pivot.md)
- [Conceptos básicos de navegación](../basics/navigation-basics.md)
- [Introducción a Fluent Design para UWP](../fluent-design-system/index.md)

