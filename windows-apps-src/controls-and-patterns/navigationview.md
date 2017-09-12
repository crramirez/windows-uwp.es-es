---
author: Jwmsft
Description: "Control que presenta la navegación de nivel superior mientras conserva el espacio de la pantalla."
title: "Vista de navegación"
ms.assetid: 
label: Navigation view
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: tpaine
doc-status: Published
ms.openlocfilehash: 98ab8a95288e5a0225a2185f7ce037e16209d173
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2017
---
# <a name="navigation-view"></a>Vista de navegación

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

> [!IMPORTANT]
> En este artículo se describe una funcionalidad que no se ha publicado aún y que puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.

El control de vista de navegación proporciona un diseño vertical común para las áreas de nivel superior de la aplicación mediante un menú de navegación contraíble. Este control está diseñado para implementar el patrón del panel de navegación, o menú hamburguesa, y adapta automáticamente su diseño a diferentes tamaños de ventana.

> **API importantes**: [Clase NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview), [Clase NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem), [Enumeración NavigationViewDisplayMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewdisplaymode)

![Ejemplo de NavigationView](images/navview_wireframe.png)


## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

NavigationView es adecuado para:

-  Aplicaciones con muchos elementos de navegación de nivel superior que son de tipo similar. Por ejemplo, una aplicación de deportes con categorías como "fútbol", "béisbol", "baloncesto", "baloncesto", "fútbol americano", etc.
-  Proporcionar una experiencia de navegación coherente entre aplicaciones. El panel de navegación debe incluir solo los elementos de navegación, no las acciones.
-  Un número de medio a alto (5-10) de categorías de navegación de nivel superior.
-  Conservación de la superficie de pantalla de ventanas menores.

La vista de navegación es solo uno de varios elementos de navegación que puedes usar. Para más información sobre patrones de navegación y otros elementos de navegación, consulta el artículo [Conceptos básicos del diseño de navegación para aplicaciones para la Plataforma universal de Windows (UWP)](../layout/navigation-basics.md).

Para obtener un ejemplo de código sobre cómo crear el patrón de panel de navegación con SplitView y ListView, descarga la [solución de navegación XAML](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/XamlNavigation) de GitHub.

## <a name="navigationview-parts"></a>Partes de NavigationView
El control se subdivide ampliamente en tres secciones: un panel de navegación a la izquierda y las áreas de encabezado y contenido a la derecha.

![Secciones de NavigationView](images/navview_sections.png)

### <a name="pane"></a>Panel

El panel de navegación puede contener:

- Elementos de navegación, en forma de [NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem), para navegar a páginas específicas
- Separadores, en forma de [NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator), para agrupar elementos de navegación
- Encabezados, en forma de [NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader), para etiquetar grupos de elementos
- Un punto de entrada opcional para [configuración de la aplicación](../app-settings/app-settings-and-data.md). Para ocultar el elemento de configuración, usa la propiedad [IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_IsSettingsVisible)
- Contenido de forma libre en el pie de página del panel, cuando se agrega a la propiedad [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_PaneFooter)

El botón de navegación integrado ("hamburguesa") permite a los usuarios abrir y cerrar el panel. En ventanas de la aplicación mayores, cuando el panel está abierto, puedes ocultar este botón usando la propiedad [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_IsPaneToggleButtonVisible).

### <a name="header"></a>Encabezado

El área de encabezado se alinea verticalmente con el botón de navegación y tiene altura fija. Su finalidad es contener el título de la página de la categoría de navegación seleccionada. El encabezado se acopla a la parte superior de la página y actúa como punto de recorte de desplazamiento para el área de contenido.

El encabezado debe estar visible cuando NavigationView se encuentra en el modo mínimo. Puedes elegir ocultar el encabezado de otros modos, que se usan en los anchos de ventana mayores. Para ello, establece la propiedad [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_AlwaysShowHeader) en **false**.

### <a name="content"></a>Contenido

El área de contenido es donde se muestra la mayor parte de la información para la categoría de navegación seleccionada. Puede contener uno o más elementos y es una buena área para la navegación de nivel inferior adicional como [Dinámica](tabs-pivot.md).

Te recomendamos márgenes de 12 píxeles en los lados de tu contenido cuando NavigationView se encuentre en el modo mínimo y márgenes de 24 píxeles en caso contrario.

## <a name="visual-style"></a>Estilo visual

<div class="microsoft-internal-note">
Las líneas rojas de este control están disponibles en [UNI](http://uni/DesignDepot.FrontEnd/#/ProductNav?t=Fluent%20Design%20System%7CControls%20%26%20Patterns%7CNavigationView).<br/><br/>
</div>

Los elementos de navegación admiten estados visuales seleccionados, deshabilitados, con el puntero encima, presionados y enfocados.

![Estados de elementos NavigationView: deshabilitado, con el puntero encima, presionado, enfocado](images/navview_item-states.png)

Cuando se cumplen los requisitos de hardware y software, NavigationView usa automáticamente el nuevo [Material acrílico](../style/acrylic.md) y [Mostrar resaltado](../style/reveal.md) en su panel.


## <a name="navigationview-modes"></a>Modos de NavigationView
El panel NavigationView puede abrirse o cerrarse, y tiene tres opciones de modo de pantalla:
-  **Mínimo** Solo el botón hamburguesa permanece fijo mientras que se muestra y se oculta el panel según sea necesario.
-  **Compacto** El panel siempre se muestra como una franja estrecha que se puede abrir hasta el ancho completo.
-  **Expandido** El panel permanece abierto a lo largo del contenido. Cuando se cierra activando el botón hamburguesa, el ancho del panel pasa a ser una franja estrecha.

De manera predeterminada, el sistema selecciona automáticamente el modo óptimo de visualización según la cantidad de espacio de pantalla disponible para el control. (Puedes invalidar esta configuración; consulta la siguiente sección para obtener más información.)

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
-  Este modo es más adecuado para pantallas medianas, como tabletas y [experiencias de 10 pies](../input-and-devices/designing-for-tv.md).
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

## <a name="overriding-the-default-adaptive-behavior"></a>Invalidación del comportamiento adaptable predeterminado

La vista de navegación cambia automáticamente su modo de pantalla en función de la cantidad de espacio en pantalla disponible para ella.
[!NOTE] NavigationView debe servir como el contenedor raíz de la aplicación; este control está diseñado para ocupar todo el ancho y el alto de la ventana de la aplicación.
Puedes invalidar los anchos en los que la vista de navegación cambia los modos de visualización usando las propiedades [CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_CompactModeThresholdWidth) y ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_ExpandedModeThresholdWidth). Ten en cuenta los siguientes escenarios que muestran cuándo es posible que quieras personalizar el comportamiento del modo de pantalla.

-  **Navegación frecuente** Si esperas que los usuarios naveguen entre las áreas de la aplicación un poco con frecuencia, piensa en la posibilidad de mantener el panel en la vista en anchos de ventana menores. Una aplicación de música con áreas de navegación de canciones/álbumes/artistas puede optar por un ancho de panel de 280 píxeles y permanecer en el modo expandido cuando el ancho de la ventana de la aplicación sea superior a 560 píxeles.
```xaml
<NavigationView OpenPaneLength=”280” CompactModeThresholdWidth="560" ExpandedModeThresholdWidth=”560”/>
```
-    **Navegación poco frecuente** Si esperas que los usuarios naveguen entre las áreas de aplicación con poca frecuencia, plantéate mantener el panel oculto para anchos de ventana mayores. Una aplicación de calculadora con varios diseños puede optar por permanecer en el modo mínimo incluso cuando la aplicación se maximiza en una pantalla de 1080 píxeles.
```xaml
<NavigationView CompactModeThresholdWidth=”1920” ExpandedModeThresholdWidth=”1920”/>
```
-    **Desambiguación de iconos** Si las áreas de navegación de la aplicación no se prestan por sí mismas para iconos significativos, evita usar el modo compacto. Una aplicación de visualización de imágenes con colecciones/álbumes/carpetas puede optar por mostrar NavigationView en el modo mínimo en anchos estrechos y medianos y en el modo expandido en el ancho amplio.
```xaml
<NavigationView CompactModeThresholdWidth=”1008”/>
```

## <a name="interaction"></a>Interacción

Cuando los usuarios pulsen en una categoría de navegación en el panel, NavigationView mostrará ese elemento como seleccionado y generará un evento [ItemInvoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_ItemInvoked). Si la pulsación da por resultado la selección de un nuevo elemento, NavigationView también generará un evento [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_SelectionChanged). Tu aplicación es responsable de actualizar el encabezado y el contenido con la información adecuada en respuesta a la interacción de este usuario. Además, te recomendamos mover el foco mediante programación desde el elemento de navegación hasta el contenido. Al establecer el foco inicial en la carga, optimizas el flujo del usuario y minimizas el número previsto de movimientos del foco del teclado.

## <a name="how-to-use-navigationview"></a>Cómo usar NavigationView

El siguiente es un ejemplo sencillo de cómo puedes incorporar NavigationView en tu aplicación.

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
                    Loaded="NavView_Loaded">
    <!-- Load NavigationViewItems in NavView_Loaded. -->

        <NavigationView.HeaderTemplate>
            <DataTemplate>
                <Grid>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto"/>
                        <ColumnDefinition/>
                    </Grid.ColumnDefinitions>
                    <TextBlock Style="{StaticResource TitleTextBlockStyle}"
                           FontSize="28"
                           VerticalAlignment="Center"
                           Margin="12,0"
                           Text="Welcome"/>
                    <CommandBar Grid.Column="1"
                            HorizontalAlignment="Right"
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

        <Frame x:Name="ContentFrame">
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
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "Apps", Icon = new SymbolIcon(Symbol.AllApps), Tag = "apps" });
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "Games", Icon = new SymbolIcon(Symbol.Video), Tag = "games" });
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "Music", Icon = new SymbolIcon(Symbol.Audio), Tag = "music" });
    NavView.MenuItems.Add(new NavigationViewItemSeparator());
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "My content", Icon = new SymbolIcon(Symbol.Folder), Tag = "content" });

    foreach (NavigationViewItem item in NavView.MenuItems)
    {
        if (item.Tag.ToString() == "play")
        {
            NavView.SelectedItem = item;
            break;
        }
    }
}

private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{
    if (args.IsSettingsInvoked == true)
    {
        ContentFrame.Navigate(typeof(SettingsPage));
    }
    else
    {
        switch ((args.InvokedItem as NavigationViewItem).Tag)
        {
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
}
```

## <a name="navigation"></a>Navegación

NavigationView no muestra automáticamente el botón Atrás en la barra de título de la aplicación ni agrega contenido a la pila de retroceso. El control no responde automáticamente a las pulsaciones del botón Atrás del software o hardware. Consulta la sección [Historial y navegación hacia atrás](../layout/navigation-history-and-backwards-navigation.md) para obtener más información acerca de este tema y de cómo agregar compatibilidad para la navegación a la aplicación.


## <a name="related-topics"></a>Temas relacionados

* [Clase NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
* [Maestro/detalles](master-details.md)
* [Control dinámico](tabs-pivot.md)
* [Conceptos básicos de navegación](../layout/navigation-basics.md)
 

 
