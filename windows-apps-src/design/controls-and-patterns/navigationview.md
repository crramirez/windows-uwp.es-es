---
author: Jwmsft
Description: NavigationView is an adaptive control that implements top-level navigation patterns for your app.
title: Vista de navegación
template: detail.hbs
ms.author: jimwalk
ms.date: 06/25/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 1c9f44f13df05aa408757a0766b2a652037707d1
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "4688154"
---
# <a name="navigation-view-preview-version"></a>Vista de navegación (versión preliminar)

> **Se trata de una versión preliminar**: este artículo describe una nueva versión del control NavigationView que aún está en desarrollo. Para usarlo, necesita la [última compilación de Windows Insider y el SDK](https://insider.windows.com/for-developers/) o la [Biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

El control NavigationView proporciona navegación de nivel superior de la aplicación. Se adaptan a una gran variedad de tamaños de pantalla es compatible con varios estilos de navegación.

> **Las API de biblioteca de la interfaz de usuario de Windows**: [clase Microsoft.UI.Xaml.Controls.NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview)

> **API de la plataforma**: [clase Windows.UI.Xaml.Controls.NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)

## <a name="get-the-windows-ui-library"></a>Obtén la biblioteca de la interfaz de usuario de Windows

Este control se incluye como parte de la biblioteca de la interfaz de usuario de Windows, un paquete de NuGet que contiene los nuevos controles y funciones de la interfaz de usuario para aplicaciones para UWP. Para obtener más información, incluidas las instrucciones de instalación, vea la [información general de la biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/). 

## <a name="navigation-styles"></a>Estilos de navegación

NavigationView admite:

**Panel de navegación izquierdo o el menú**

![expandidas el panel de navegación](images/displaymode-left.png)

**Panel de navegación superior o menú**

![navegación superior](images/displaymode-top.png)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

NavigationView es un control de navegación adaptable que funciona bien para:

- Proporcionar una experiencia de navegación coherente en toda la aplicación.
- Conservar el espacio en pantalla en ventanas más pequeñas.
- Organizar el acceso a muchas de las categorías de navegación.

Para otros controles de navegación, consulta los [conceptos básicos del diseño de navegación](../basics/navigation-basics.md).

Si la navegación requiere un comportamiento más complejo que no es compatible con NavigationView, quizá te interese el patrón [Panel maestro y detalles](master-details.md) en su lugar.

:::row:::
    :::column:::
        ![Some image](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    :::column span="2":::
        **XAML Controls Gallery**<br>
        If you have the XAML Controls Gallery app installed, click <a href="xamlcontrolsgallery:/item/NavigationView">here</a> to open the app and see NavigationView in action.

        <a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a><br>
        <a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Get the source code (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="display-modes"></a>Modos de visualización

NavigationView se puede establecer en los modos de visualización diferentes, a través de la `PaneDisplayMode` propiedad:

:::row:::
    :::column:::
    ### <a name="left"></a>Izquierda
    Muestra un panel de camino izquierdo expandido.
    :::column-end:::
    :::column span="2":::
    ![panel de navegación izquierdo expandido](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

Te recomendamos de navegación izquierdo cuando:

- Tiene un número de medio a alto (5-10) de categorías de navegación de nivel superior igualmente importante.
- Que quieras categorías de navegación muy destacada con menos espacio para otro contenido de la aplicación.

:::row:::
    :::column:::
    ### <a name="top"></a>Top
    Muestra una parte superior coloca el panel.
    :::column-end:::
    :::column span="2":::
    ![navegación superior](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

Te recomendamos de navegación superior cuando:

- Tiene 5 o menos categorías de navegación de nivel superior igualmente importante, que las categorías de navegación de nivel superior adicionales que terminan en la lista desplegable de desbordamiento menú se consideran menos importantes.
- Debes mostrar todas las opciones de navegación en pantalla.
- Desean más espacio para el contenido de la aplicación.
- Iconos claramente no describen las categorías de navegación de la aplicación.

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    Muestra una franja thin con iconos a la izquierda.
    :::column-end:::
    :::column span="2":::
    ![panel de navegación compacto](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    Muestra solo el botón de menú.
    :::column-end:::
    :::column span="2":::
    ![panel de navegación mínimo](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>Automático

![comportamiento adaptable de GIF leftnav predeterminado](images/displaymode-auto.png)

Se adapta entre LeftMinimal en pantallas pequeñas, LeftCompact en pantallas medianas e izquierda en pantallas de gran tamaño. Consulta la sección de [comportamiento adaptable](#adaptive-behavior) para obtener más información.

## <a name="anatomy"></a>Anatomía

<b>Barra de navegación izquierda</b><br>

![secciones de NavigationView izquierdas](images/leftnav-anatomy.png)

<b>Navegación superior</b><br>

![secciones de NavigationView superiores](images/topnav-anatomy.png)

## <a name="pane"></a>Panel

El panel puede colocarse en la parte superior o a la izquierda, a través de la `PanePosition` propiedad.

Esta es la Anatomía de panel detallada de las posiciones de panel izquierdo y superior:

<b>Barra de navegación izquierda</b><br>

![Anatomía de NavigationView](images/navview-pane-anatomy-vertical.png)

1. Botón de menú
1. Elementos de navegación
1. Separadores
1. Encabezados
1. AutoSuggestBox (opcional)
1. Botón de configuración (opcional)

<b>Navegación superior</b><br>

![Anatomía de NavigationView](images/navview-pane-anatomy-horizontal.png)

1. Encabezados
1. Elementos de navegación
1. Separadores
1. AutoSuggestBox (opcional)
1. Botón de configuración (opcional)

Aparece el botón Atrás en la esquina superior izquierda del panel, pero NavigationView no agrega automáticamente contenido a la pila de retroceso. Para habilitar la navegación hacia atrás, consulta el [hacia atrás navegación](#backwards-navigation) sección.

El panel de NavigationView también puede contener:

1. Elementos de navegación, en forma de [NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem), para navegar a páginas específicas.
2. Separadores, en forma de [NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator), para agrupar elementos de navegación. Establece la propiedad [Opacity](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator.opacity) en 0 para representar el separador de espacio.
3. Encabezados, en forma de [NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader), para etiquetar grupos de elementos.
4. Un opcional [AutoSuggestBox](auto-suggest-box.md) para permitir la búsqueda de nivel de la aplicación.
5. Un punto de entrada opcional para la [configuración de la aplicación](../app-settings/app-settings-and-data.md). Para ocultar el elemento de configuración, usa la propiedad [IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) .

El panel izquierdo contiene lo siguiente:

6. Botón de menú para activar o desactivar el panel abierto y cierre. En ventanas de la aplicación mayores, cuando el panel está abierto, puedes ocultar este botón usando la propiedad [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible).

### <a name="pane-footer"></a>El pie de página de panel

Contenido de forma libre en el pie de página del panel, cuando se agrega a la propiedad [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter)

:::row:::
    :::column:::
    <b>Barra de navegación izquierda</b><br>
    ![Menú de navegación izquierdo del pie de página de panel](images/navview-freeform-footer-left.png)<br>
    :::column-end:::
    :::column:::
     <b>Navegación superior</b><br>
    ![Navegación superior del encabezado de panel](images/navview-freeform-footer-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="pane-header"></a>Encabezado del panel

Contenido de forma libre en el encabezado del panel, cuando se agrega a la propiedad [PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader)

:::row:::
    :::column:::
    <b>Barra de navegación izquierda</b><br>
    ![Menú de navegación izquierdo del encabezado de panel](images/navview-freeform-header-left.png)<br>
    :::column-end:::
    :::column:::
     <b>Navegación superior</b><br>
    ![Navegación superior del encabezado de panel](images/navview-freeform-header-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="pane-content"></a>Contenido de panel

Contenido de forma libre en el panel, cuando se agrega a la propiedad [PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent)

:::row:::
    :::column:::
    <b>Barra de navegación izquierda</b><br>
    ![Navegación de panel personalizado contentleft](images/navview-freeform-pane-left.png)<br>
    :::column-end:::
    :::column:::
     <b>Navegación superior</b><br>
    ![Panel navegación superior contenido personalizado](images/navview-freeform-pane-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="visual-style"></a>Estilo visual

Cuando se cumplen los requisitos de hardware y software, NavigationView usa automáticamente el [material acrílico](../style/acrylic.md) en su panel y [Mostrar resaltado](../style/reveal.md) solo en el panel izquierdo.

## <a name="header"></a>Encabezado

![imagen genérica navview del área de encabezado](images/nav-header.png)

El área de encabezado se alinea verticalmente con el botón de navegación en la posición del panel izquierdo y se encuentra debajo del panel en la posición de panel superior. Tiene una altura fija de 52 px. Su finalidad es contener el título de la página de la categoría de navegación seleccionada. El encabezado se acopla a la parte superior de la página y actúa como punto de recorte de desplazamiento para el área de contenido.

El encabezado debe estar visible cuando NavigationView está en modo de presentación mínima. Puedes elegir ocultar el encabezado de otros modos, que se usan en los anchos de ventana mayores. Para ello, establece la propiedad [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) en **false**.

## <a name="content"></a>Contenido

![imagen genérica navview del área de contenido](images/nav-content.png)

El área de contenido es donde se muestra la mayor parte de la información para la categoría de navegación seleccionada.

Te recomendamos márgenes de 12 píxeles en el área de tu contenido cuando NavigationView se encuentre en el modo mínimo y márgenes de 24 píxeles en caso contrario.

## <a name="adaptive-behavior"></a>Comportamiento adaptable

NavigationView cambia automáticamente su modo de pantalla en función de la cantidad de espacio en pantalla disponible para ella. Sin embargo, es posible que quieras personalizar el comportamiento de modo de presentación adaptable.

### <a name="default"></a>Predeterminado

El comportamiento adaptable predeterminado de NavigationView es mostrar un panel izquierdo expandido en anchos de ventana grande, un panel de navegación izquierdo de solo icono en anchos de ventana mediana y un botón de menú hamburguesa en anchos de ventana pequeña. Para obtener más información sobre los tamaños de ventana para un comportamiento adaptativo, consulta [los tamaños de pantalla y puntos de interrupción](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

![comportamiento adaptable de GIF leftnav predeterminado](images/displaymode-auto.png)

```xaml
<NavigationView />
```

### <a name="minimal"></a>Mínimo

Un patrón común que adaptable segundo es usar un panel de la izquierda expandido en anchos de ventana grande y un menú de hamburguesa en ambos anchos de ventana pequeñas y medianas.

![comportamiento adaptable de GIF leftnav 2](images/adaptive-behavior-minimal.png)

```xaml
<NavigationView CompactModeThresholdWidth="1008" ExpandedModeThresholdWidth="1007" />
```

Te recomendamos esto cuando:

- Desean más espacio para el contenido de la aplicación en los anchos de ventana más pequeños.
- Las categorías de navegación no se puede representar claramente con iconos.

### <a name="compact"></a>Compacto

Un patrón común que adaptable tercero es usar un panel de la izquierda expandido en anchos de ventana grande y un panel de navegación izquierdo de solo icono en ambos anchos de ventana pequeñas y medianas. Un buen ejemplo de esto es la aplicación de correo.

![comportamiento adaptable de GIF leftnav 3](images/adaptive-behavior-compact.png)

```xaml
<NavigationView CompactModeThresholdWidth="0" ExpandedModeThresholdWidth="1007" />
```

Te recomendamos esto cuando:

- Es importante para siempre mostrar todas las opciones de navegación en pantalla.
- las categorías de navegación se pueden representar claramente con iconos.

### <a name="no-adaptive-behavior"></a>Ningún comportamiento adaptable

A veces no es posible que quieras ningún comportamiento adaptable en absoluto. Puedes establecer el panel para estar siempre expandido, siempre compacto o siempre mínima.

![comportamiento adaptable de GIF leftnav 4](images/adaptive-behavior-none.png)

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

### <a name="top-to-left-navigation"></a>La parte superior a la izquierda

Te recomendamos que uses de navegación superior en tamaños de ventana grande y navegación izquierda en pequeñas tamaños de ventana cuando:

- Tienen un conjunto de categorías de navegación de nivel superior importante igualmente mostrarán juntos, si una categoría en este conjunto no cabe en pantalla, contraer a de navegación izquierdo para darles misma importancia.
- Desea conservar como contenido mucho espacio como sea posible en tamaños de ventana pequeña.

A continuación mostramos un ejemplo:

![comportamiento adaptable de navegación izquierdo o superior de GIF 1](images/navigation-top-to-left.png)

```xaml
<Grid >
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                </VisualState.StateTriggers>

                <VisualState.Setters>
                    <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>

    <NavigationView x:Name="NavigationViewControl" >
        <NavigationView.MenuItems>
            <NavigationViewItem Content="A" x:Name="A" />
            <NavigationViewItem Content="B" x:Name="B" />
            <NavigationViewItem Content="C" x:Name="C" />
        </NavigationView.MenuItems>
    </NavigationView>
</Grid>

```

A veces, las aplicaciones necesitan enlazar datos diferentes a la sección superior y el panel izquierdo. A menudo en el panel izquierdo se incluye más elementos de navegación.

A continuación mostramos un ejemplo:

![comportamiento adaptable de navegación izquierdo o superior de GIF 2](images/navigation-top-to-left2.png)

```xaml
<Page >
    <Page.Resources>
        <DataTemplate x:name="navItem_top_temp" x:DataType="models:Item">
            <NavigationViewItem Background= Icon={x:Bind TopIcon}, Content={x:Bind TopContent}, Visibility={x:Bind TopVisibility} />
        </DataTemplate>

        <DataTemplate x:name="navItem_temp" x:DataType="models:Item">
            <NavigationViewItem Icon={x:Bind Icon}, Content={x:Bind Content}, Visibility={x:Bind Visibility} />
        </DataTemplate>
        
        <services:NavViewDataTemplateSelector x:Key="navview_selector" 
              NavItemTemplate="{StaticResource navItem_temp}" 
              NavItemTopTemplate="{StaticResource navItem_top_temp}" 
              NavPaneDisplayMode="{x:Bind NavigationViewControl.PaneDisplayMode}">
        </services:NavViewDataTemplateSelector>
    </Page.Resources>

    <Grid >
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <NavView x:Name='NavigationViewControl' MenuItemsSource={x:Bind items}   
                 PanePosition = "Top" MenuItemTemplateSelector="navview_selector" />
    </Grid>
</Page>

```

```csharp
ObservableCollection<Item> items = new ObservableCollection<Item>();
items.Add(new Item() {
    Content = "Aa",
    TopContent ="A",
    Icon = new BitmapIcon() { UriSource = new Uri("ms-appx:///testimage.jpg") },
    TopIcon = new BitmapIcon(),
    ItemVisibility = Visibility.Visible,
    TopItemVisiblity = Visibility.Visible 
});
items.Add(new Item() {
    Content = "Bb",
    TopContent = "B",
    Icon = new BitmapIcon() { UriSource = new Uri("ms-appx:///testimage.jpg") },
    TopIcon = new BitmapIcon(),
    ItemVisibility = Visibility.Visible,
    TopItemVisiblity = Visibility.Visible 
});
items.Add(new Item() {
    Content = "Cc",
    TopContent = "C",
    Icon = new BitmapIcon() { UriSource = new Uri("ms-appx:///testimage.jpg") },
    TopIcon = new BitmapIcon(),
    ItemVisibility = Visibility.Visible,
    TopItemVisiblity = Visibility.Visible 
});

public class NavViewDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate NavItemTemplate { get; set; }

    public DataTemplate NavItemTopTemplate { get; set; }    

    public NavigationViewPaneDisplayMode NavPaneDisplayMode { get; set; }

    protected override DataTemplate SelectTemplateCore(object item)
    {
        Item currItem = item as Item;
        if (NavPaneDisplayMode == NavigationViewPanePosition.Top)
            return NavItemTopTemplate;
        else 
            return NavItemTemplate;
    }   

}

```

## <a name="interaction"></a>Interacción

Cuando los usuarios pulsen en un elemento de navegación en el panel, NavigationView mostrará ese elemento como seleccionado y generará un evento [ItemInvoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked). Si la pulsación da por resultado la selección de un nuevo elemento, NavigationView también generará un evento [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged).

Tu aplicación es responsable de actualizar el encabezado y el contenido con la información adecuada en respuesta a la interacción de este usuario. Además, te recomendamos mover el [foco](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.FocusState) mediante programación desde el elemento de navegación hasta el contenido. Al establecer el foco inicial en la carga, optimizas el flujo del usuario y minimizas el número previsto de movimientos del foco del teclado.

### <a name="tabs"></a>Pestañas

En el modelo de pestañas, están vinculados selección y foco. Una acción que normalmente turnos el foco se también cambiará selección. En el ejemplo siguiente, direccionales derecha mueve el indicador de selección en la pantalla hasta lupa. Puedes lograrlo estableciendo la propiedad [SelectionFollowsFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.selectionfollowsfocus) en Enabled.

![captura de pantalla de solo texto navview superior](images/nav-tabs.png)

Este es el XAML de ejemplo para que:

```xaml
<NavigationView PanePosition="Top" SelectionFollowsFocus="Enabled" >
   <NavigationView.MenuItems>
        <NavigationViewItem Content="Display" />
        <NavigationViewItem Content="Magnifier"  />
        <NavigationViewItem Content="Keyboard" />
    </NavigationView.MenuItems>
</NavigationView>

```

Para intercambiar el contenido al cambiar la selección de la pestaña, puedes usar método de [NavigateWithOptions](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.NavigateToType) del marco con FrameNavigationOptions.IsNavigationStackEnabled establecido en False y NavigateOptions.TransitionInfoOverride se establece en el apropiado al lado animación de diapositiva. Por ejemplo, consulta el siguiente [ejemplo de código](#code-example) .

Si deseas cambiar el estilo predeterminado, puedes invalidar propiedad de [MenuItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.menuitemcontainerstyle) de NavigationView. También puedes establecer la propiedad [MenuItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.menuitemtemplate) para especificar una plantilla de datos diferentes.

## <a name="backwards-navigation"></a>Navegación hacia atrás

NavigationView tiene un botón Atrás integrado, que se puede habilitar con las siguientes propiedades:

- [**IsBackButtonVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible) es una enumeración NavigationViewBackButtonVisible y "Auto" de manera predeterminada. Se utiliza para mostrar u ocultar el botón Atrás. Cuando el botón no está visible, se contraerá el espacio para dibujar el botón Atrás.
- [**IsBackEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled) es falso de manera predeterminada y puede usarse para alternar los estados del botón Atrás.
- [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) se activa cuando un usuario hace clic en el botón Atrás.
    - En el modo mínimo/compacto, cuando NavigationView.Pane está abierto como un control flotante, al hacer clic en el botón Atrás se cerrará el panel y se activará en su lugar el evento **PaneClosing**.
    - No se activará si IsBackEnabled es falso.

:::row:::
    :::column:::
    <b>Barra de navegación izquierda</b><br>
    ![Botón Atrás de NavigationView de barra de navegación izquierda](images/leftnav-back.png)
    :::column-end:::
    :::column:::
     <b>Navegación superior</b><br>
    ![Botón Atrás de NavigationView de navegación superior](images/topnav-back.png)
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>Ejemplo de código

> [!NOTE]
> NavigationView debe servir como el contenedor raíz de la aplicación, ya que este control está diseñado para ocupar todo el ancho y el alto de la ventana de la aplicación.
Puedes invalidar los anchos en los que la vista de navegación cambia los modos de visualización usando las propiedades [CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.CompactModeThresholdWidth) y [ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ExpandedModeThresholdWidth).

El siguiente es un ejemplo de principio a fin de cómo puedes incorporar NavigationView con un panel de navegación superior a tamaños de ventana grande y un panel de navegación izquierdo de tamaños de ventana pequeña.

En este ejemplo, esperamos que los usuarios finales con frecuencia seleccionar nuevas de las categorías de navegación, por lo que te:

- Establece la propiedad [SelectionFollowsFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PanePostion) en Enabled
- Usa las navegaciones de marco que no se agregan a la pila de navegación.
- Conservar el valor predeterminado en la propiedad [ShoulderNavigationEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PanePostion) , que se usa para indicar si reboteadores izquierdo/derecho en un controlador para juegos navegar por las categorías de navegación de nivel superior de la aplicación. El valor predeterminado es "WhenSelectionFollowsFocus". Los otros valores posibles son "Siempre" y "Nunca".

También se muestra cómo implementar la navegación con el botón Atrás de NavigationView hacia atrás.

Esta es una grabación de la muestra que indica:

![Muestra de principio a fin de NavigationView](images/nav-code-example.gif)

Este es el código de ejemplo:

> [!NOTE]
> Si estás usando la [Biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/), tendrás que agregar una referencia para el Kit de herramientas: `xmlns:controls="using:Microsoft.UI.Xaml.Controls"`.

```xaml
<Page
    x:Class="NavigationViewSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:NavigationViewSample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="{x:Bind NavView.CompactModeThresholdWidth}" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="NavView.PaneDisplayMode" Value="Top"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <NavigationView x:Name="NavView"
                    SelectionFollowsFocus="Enabled"
                    ItemInvoked="NavView_ItemInvoked"
                    IsSettingsVisible="True"
                    Loaded="NavView_Loaded"
                    BackRequested="NavView_BackRequested"
                    Header="Welcome">

            <NavigationView.MenuItems>
                <NavigationViewItem Content="Home" x:Name="home" Tag="home">
                    <NavigationViewItem.Icon>
                        <FontIcon Glyph="&#xE10F;"/>
                    </NavigationViewItem.Icon>
                </NavigationViewItem>
                <NavigationViewItemSeparator/>
                <NavigationViewItemHeader Content="Main pages"/>
                <NavigationViewItem Icon="AllApps" Content="Apps" x:Name="apps" Tag="apps"/>
                <NavigationViewItem Icon="Video" Content="Games" x:Name="games" Tag="games"/>
                <NavigationViewItem Icon="Audio" Content="Music" x:Name="music" Tag="music"/>
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

            <Frame x:Name="ContentFrame" Margin="24"/>

        </NavigationView>
    </Grid>
</Page>
```

> [!NOTE]
> Si estás usando la [Biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/), tendrás que agregar una referencia para el Kit de herramientas: `using MUXC = Microsoft.UI.Xaml.Controls;`.

```csharp
private Type currentPage;

// List of ValueTuple holding the Navigation Tag and the relative Navigation Page 
private readonly IList<(string Tag, Type Page)> _pages = new List<(string Tag, Type Page)>
{
    ("home", typeof(HomePage)),
    ("apps", typeof(AppsPage)),
    ("games", typeof(GamesPage)),
    ("music", typeof(MusicPage)),
};

private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    // You can also add items in code behind
    NavView.MenuItems.Add(new NavigationViewItemSeparator());
    NavView.MenuItems.Add(new NavigationViewItem
    {
        Content = "My content",
        Icon = new SymbolIcon(Symbol.Folder),
        Tag = "content"
    });
    _pages.Add(("content", typeof(MyContentPage)));

    ContentFrame.Navigated += On_Navigated;

    // NavView doesn't load any page by default: you need to specify it
    NavView_Navigate("home");

    // Add keyboard accelerators for backwards navigation
    var goBack = new KeyboardAccelerator { Key = VirtualKey.GoBack };
    goBack.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(goBack);

    // ALT routes here
    var altLeft = new KeyboardAccelerator
    {
        Key = VirtualKey.Left,
        Modifiers = VirtualKeyModifiers.Menu
    };
    altLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(altLeft);
}

private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{
    if (args.InvokedItem == null)
        return;

    if (args.IsSettingsInvoked)
        ContentFrame.Navigate(typeof(SettingsPage));
    else
    {
        // Getting the Tag from Content (args.InvokedItem is the content of NavigationViewItem)
        var navItemTag = NavView.MenuItems
            .OfType<NavigationViewItem>()
            .First(i => args.InvokedItem.Equals(i.Content))
            .Tag.ToString();

        NavView_Navigate(navItemTag);
    }
}

private void NavView_Navigate(string navItemTag)
{
    var item = _pages.First(p => p.Tag.Equals(navItemTag));
    if (currentPage == item.Page)
          return;
    ContentFrame.Navigate(item.Page);

    currentPage = item.Page;
}

private void NavView_BackRequested(NavigationView sender, NavigationViewBackRequestedEventArgs args) => On_BackRequested();

private void BackInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}

private bool On_BackRequested()
{
    if (!ContentFrame.CanGoBack)
        return false;

    // Don't go back if the nav pane is overlayed
    if (NavView.IsPaneOpen &&
        (NavView.DisplayMode == NavigationViewDisplayMode.Compact ||
        NavView.DisplayMode == NavigationViewDisplayMode.Minimal))
        return false;

    ContentFrame.GoBack();
    return true;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag
        NavView.SelectedItem = (NavigationViewItem)NavView.SettingsItem;
    }
    else
    {
        var item = _pages.First(p => p.Page == e.SourcePageType);

        NavView.SelectedItem = NavView.MenuItems
            .OfType<NavigationViewItem>()
            .First(n => n.Tag.Equals(item.Tag));
    }
}
```

## <a name="customizing-backgrounds"></a>Personalización de fondos

Para cambiar el fondo del área principal de NavigationView, establece su propiedad `Background` en tu pincel preferido.

El fondo del panel muestra acrílico en la aplicación cuando NavigationView está en la parte superior, mínima, o el modo compacto. Para actualizar este comportamiento o personalizar la apariencia del acrílico del panel, modifica los dos recursos de tema sobrescribiéndolos en tu App.xaml.

```xaml
<Application.Resources>
    <ResourceDictionary>
        <AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
        BackgroundSource="Backdrop" TintColor="Yellow" TintOpacity=".6"/>
        <AcrylicBrush x:Key="NavigationViewTopPaneBackground"
        BackgroundSource="Backdrop" TintColor="Yellow" TintOpacity=".6"/>
        <AcrylicBrush x:Key="NavigationViewExpandedPaneBackground"
        BackgroundSource="HostBackdrop" TintColor="Orange" TintOpacity=".8"/>
    </ResourceDictionary>
</Application.Resources>
```

## <a name="scroll-content-under-top-pane"></a>Contenido de desplazamiento en el panel superior

Para un aspecto impecable + sensación, si la aplicación tiene las páginas que usan un ScrollViewer y el panel de navegación está arriba situado, se recomienda tener el desplazamiento de contenido debajo del panel de navegación superior. Esto te ofrece un tipo de encabezado permanente del comportamiento de la aplicación.

Esto se logra estableciendo la propiedad [CanContentRenderOutsideBounds](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.cancontentrenderoutsidebounds) en el ScrollViewer relevante en true.

![navview desplazamiento navpane](images/nav-scroll-content.png)

Si la aplicación tiene contenido de desplazamiento muy larga, puedes tener en cuenta la incorporación de los encabezados permanentes que asociar al panel de navegación superior y forman una superficie suave. 

![encabezado permanente de desplazamiento navview](images/nav-scroll-stickyheader.png)

Puedes lograrlo estableciendo la propiedad [ContentOverlay](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ContentOverlay) en NavigationView. 

En ocasiones, si el usuario se desplaza hacia abajo, puedes ocultar el panel de navegación, que se logra estableciendo la propiedad [IsPaneVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ContentOverlay) en NavigationView en "false".

![navview desplazamiento ocultar nav](images/nav-scroll-hidepane.png)

## <a name="related-topics"></a>Temas relacionados

- [Clase NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [Maestro/detalles](master-details.md)
- [Control dinámico](tabs-pivot.md)
- [Conceptos básicos de navegación](../basics/navigation-basics.md)
- [Introducción a Fluent Design para UWP](../fluent-design-system/index.md)
