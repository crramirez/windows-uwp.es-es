---
author: serenaz
Description: NavigationView is an adaptive control that implements top-level navigation patterns for your app.
title: Vista de navegación
template: detail.hbs
ms.author: sezhen
ms.date: 08/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4c0857005d584b1fde0eb52a6ab0ef5ec29eaf44
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2018
ms.locfileid: "2886349"
---
# <a name="navigation-view-preview-version"></a>Vista de navegación (versión preliminar)

> **Ésta es una versión de vista previa**: en este artículo se describe una nueva versión del control NavigationView que aún está en desarrollo. Para que lo utilice ahora, necesita la [generación de información confidencial de Windows y SDK más recientes](https://insider.windows.com/for-developers/) o la [Biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

El control de NavigationView proporciona navegación de nivel superior para la aplicación. Se adaptan a una gran variedad de tamaños de pantalla es compatible con varios estilos de navegación.

> **API de biblioteca de la interfaz de usuario de Windows**: [clase de Microsoft.UI.Xaml.Controls.NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview)

> **API de la plataforma**: [clase de Windows.UI.Xaml.Controls.NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)

## <a name="get-the-windows-ui-library"></a>Obtener la biblioteca de la interfaz de usuario de Windows

Este control se incluye como parte de la biblioteca de la interfaz de usuario de Windows, un paquete de NuGet que contiene los nuevos controles y características de la interfaz de usuario para las aplicaciones UWP. Para obtener más información, incluidas las instrucciones de instalación, vea la [Introducción a la biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/). 

## <a name="navigation-styles"></a>Estilos de navegación

NavigationView admite:

**Panel de navegación izquierdo o menú**

![panel del panel de navegación expandido](images/displaymode-left.png)

**Panel de navegación superior o menú**

![navegación superior](images/displaymode-top.png)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

NavigationView es un control de navegación adaptable que funciona bien para:

- Proporcionar una experiencia de exploración coherente en toda la aplicación.
- Conservación de pantalla bienes inmuebles en ventanas más pequeñas.
- Organizar el acceso a muchas de las categorías de navegación.

Para otros controles de navegación, vea [conceptos básicos del diseño de navegación](../basics/navigation-basics.md).

Si la navegación requiere un comportamiento más complejo que no es compatible con NavigationView, quizá te interese el patrón [Panel maestro y detalles](master-details.md) en su lugar.

:::row:::
    :::column:::
        ![Algunas imágenes](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    ::: extensión de columna = "2"::: **Galería de controles de XAML**<br>
        Si tiene la aplicación de la Galería de controles de XAML instalada, haga clic <a href="xamlcontrolsgallery:/item/NavigationView">aquí</a> para abrir la aplicación y vea NavigationView en acción.

        <a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a><br>
        <a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Get the source code (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="display-modes"></a>Modos de presentación

NavigationView se puede establecer en distintos modos de presentación, a través de la `PaneDisplayMode` (propiedad):

:::row:::
    :::column:::
    ### Left
    Displays an expanded left positioned pane.
    :::column-end:::
    :::column span="2":::
    ![left nav pane expanded](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

Se recomienda navegación izquierda cuando:

- Tiene un número de medio a alto (5-10) de categorías de navegación de nivel superior igualmente importante.
- Desean categorías de navegación muy importantes con menos espacio para otro contenido de la aplicación.

:::row:::
    :::column:::
    ### Top
    Displays a top positioned pane.
    :::column-end:::
    :::column span="2":::
    ![top navigation](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

Se recomienda navegación superior cuando:

- Tiene 5 o menos categorías de navegación de nivel superior igualmente importante, por ejemplo, que las categorías de navegación de nivel superior adicionales que terminan en la lista desplegable de desbordamiento menú se consideran menos importante.
- Necesario mostrar todas las opciones de navegación en pantalla.
- Desean más espacio para el contenido de la aplicación.
- Los iconos no pueden describir claramente las categorías de navegación de la aplicación.

:::row:::
    :::column:::
    ### LeftCompact
    Displays a thin sliver with icons on the left.
    :::column-end:::
    :::column span="2":::
    ![nav pane compact](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### LeftMinimal
    Displays only the menu button.
    :::column-end:::
    :::column span="2":::
    ![nav pane minimal](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>Automático

![comportamiento adaptable de GIF leftnav predeterminado](images/displaymode-auto.png)

Se adapta entre LeftMinimal en pantallas pequeñas, LeftCompact en pantallas medianas e izquierda en pantallas de gran tamaño. Vea la sección [comportamiento adaptable](#adaptive-behavior) para obtener más información.

## <a name="anatomy"></a>Anatomía

<b>Panel de navegación izquierdo</b><br>

![en las secciones NavigationView izquierdas](images/leftnav-anatomy.png)

<b>Panel de navegación superior</b><br>

![en las secciones superiores NavigationView](images/topnav-anatomy.png)

## <a name="pane"></a>Panel

El panel puede situado en la parte superior o a la izquierda, a través de la `PanePosition` (propiedad).

Esta es la Anatomía de panel detallada para las posiciones de panel superior e izquierdo:

<b>Panel de navegación izquierdo</b><br>

![Anatomía de NavigationView](images/navview-pane-anatomy-vertical.png)

1. Botón de menú
1. Elementos de navegación
1. Separadores
1. Encabezados
1. AutoSuggestBox (opcional)
1. Botón de configuración (opcional)

<b>Panel de navegación superior</b><br>

![Anatomía de NavigationView](images/navview-pane-anatomy-horizontal.png)

1. Encabezados
1. Elementos de navegación
1. Separadores
1. AutoSuggestBox (opcional)
1. Botón de configuración (opcional)

Aparece el botón Atrás en la esquina superior izquierda del panel, pero NavigationView no se agrega automáticamente contenido a la pila de retroceso. Para habilitar la navegación con versiones anteriores, vea el [con versiones anteriores navegación](#backwards-navigation) sección.

También puede contener el panel NavigationView:

1. Elementos de navegación, en el formulario de [NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem), para navegar por las páginas específicas.
2. Separadores, en el formulario de [NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator), para agrupar los elementos de navegación. Establezca la propiedad [opacidad](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator.opacity) a 0 para representar el separador de como espacio.
3. Encabezados, en el formulario de [NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader), para el etiquetado de grupos de elementos.
4. Un opcional [AutoSuggestBox](auto-suggest-box.md) para permitir la búsqueda en el nivel de aplicación.
5. Un punto de entrada opcional para la [configuración de la aplicación](../app-settings/app-settings-and-data.md). Para ocultar el elemento de configuración, utilice la propiedad [IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) .

El panel izquierdo contiene:

6. Botón de menú para alternar el panel Abrir y cerrar. En ventanas de la aplicación mayores, cuando el panel está abierto, puedes ocultar este botón usando la propiedad [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible).

### <a name="pane-footer"></a>Pie de página de panel

Contenido de forma libre en el pie de página del panel, cuando se agrega a la propiedad [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter)

:::row:::
    :::column:::
    <b>Panel de navegación izquierdo</b><br>
    ![Panel de navegación izquierdo del pie de página de panel](images/navview-freeform-footer-left.png)<br>
    :::column-end:::
    :::column:::
     <b>Panel de navegación superior</b><br>
    ![Panel de navegación superior del encabezado de panel](images/navview-freeform-footer-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="pane-header"></a>Encabezado del panel

Contenido de forma libre en el encabezado del panel, cuando se agrega a la propiedad [PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader)

:::row:::
    :::column:::
    <b>Panel de navegación izquierdo</b><br>
    ![Panel de navegación izquierdo del encabezado de panel](images/navview-freeform-header-left.png)<br>
    :::column-end:::
    :::column:::
     <b>Panel de navegación superior</b><br>
    ![Panel de navegación superior del encabezado de panel](images/navview-freeform-header-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="pane-content"></a>Contenido de panel

Contenido de forma libre en el panel, cuando se agrega a la propiedad [PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent)

:::row:::
    :::column:::
    <b>Panel de navegación izquierdo</b><br>
    ![Panel de navegación de panel personalizado contentleft](images/navview-freeform-pane-left.png)<br>
    :::column-end:::
    :::column:::
     <b>Panel de navegación superior</b><br>
    ![Panel navegación superior contenido personalizado](images/navview-freeform-pane-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="visual-style"></a>Estilo visual

Cuando se cumplen los requisitos de hardware y software, NavigationView utiliza automáticamente el [material acrílico](../style/acrylic.md) en su panel y [revelar resaltado](../style/reveal.md) sólo en el panel izquierdo.

## <a name="header"></a>Encabezado

![imagen genérica de navview del área de encabezado](images/nav-header.png)

El área de encabezado se alinea verticalmente con el botón de navegación en la posición del panel izquierdo y se encuentra debajo del panel en la posición del panel superior. Tiene un alto fijo de 52 px. Su finalidad es contener el título de la página de la categoría de navegación seleccionada. El encabezado se acopla a la parte superior de la página y actúa como punto de recorte de desplazamiento para el área de contenido.

El encabezado debe ser visible cuando NavigationView está en modo de presentación mínima. Puedes elegir ocultar el encabezado de otros modos, que se usan en los anchos de ventana mayores. Para ello, establece la propiedad [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) en **false**.

## <a name="content"></a>Contenido

![imagen genérica de navview del área de contenido](images/nav-content.png)

El área de contenido es donde se muestra la mayor parte de la información para la categoría de navegación seleccionada.

Te recomendamos márgenes de 12 píxeles en el área de tu contenido cuando NavigationView se encuentre en el modo mínimo y márgenes de 24 píxeles en caso contrario.

## <a name="adaptive-behavior"></a>Comportamiento adaptable

NavigationView cambia automáticamente su modo de pantalla en función de la cantidad de espacio en pantalla disponible para ella. Sin embargo, es posible que desee personalizar el comportamiento de modo de presentación adaptable.

### <a name="default"></a>Predeterminado

El comportamiento adaptable predeterminado de NavigationView es mostrar un panel izquierdo expandido en anchos de ventana grande, un panel de barra de navegación izquierda de solo icono en anchos de ventana mediana y un botón de menú hamburguesa en anchos de ventana pequeña. Para obtener más información acerca de los tamaños de ventana para el comportamiento adaptable, vea [puntos de interrupción y tamaños de pantalla](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

![comportamiento adaptable de GIF leftnav predeterminado](images/displaymode-auto.png)

```xaml
<NavigationView />
```

### <a name="minimal"></a>Mínimo

Un segundo modelo adaptable común consiste en usar un panel izquierdo expandido en anchos de ventana grande y un menú hamburguesa en ambos anchos de ventana pequeñas y medianas.

![GIF leftnav adaptable comportamiento 2](images/adaptive-behavior-minimal.png)

```xaml
<NavigationView CompactModeThresholdWidth="1008" ExpandedModeThresholdWidth="1007" />
```

Se recomienda esto cuando:

- Desean más espacio para el contenido de la aplicación en los anchos de ventana más pequeños.
- Las categorías de navegación no se puede representar claramente con iconos.

### <a name="compact"></a>Compacto

Un tercer modelo adaptable común consiste en usar un panel izquierdo expandido en anchos de ventana grande y un panel de barra de navegación izquierda de solo icono en ambos anchos de ventana pequeñas y medianas. Un buen ejemplo de esto es la aplicación de correo.

![GIF leftnav adaptable comportamiento 3](images/adaptive-behavior-compact.png)

```xaml
<NavigationView CompactModeThresholdWidth="0" ExpandedModeThresholdWidth="1007" />
```

Se recomienda esto cuando:

- Es importante mostrar siempre todas las opciones de navegación en pantalla.
- las categorías de navegación se pueden representar claramente con iconos.

### <a name="no-adaptive-behavior"></a>Ningún comportamiento adaptable

En ocasiones, es posible que no desee cualquier comportamiento adaptable en absoluto. Puede configurar el panel a estar siempre expandido, siempre compact o siempre mínima.

![GIF leftnav adaptable comportamiento 4](images/adaptive-behavior-none.png)

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

### <a name="top-to-left-navigation"></a>Arriba a la izquierda

Se recomienda usar la navegación superior en tamaños de ventana grandes y pequeñas de navegación izquierdo tamaños de ventana cuando:

- Tiene un conjunto de categorías de navegación de nivel superior importante igualmente a se muestran a la vez, tal que si una categoría en este conjunto no se engloban en pantalla, se contraer para navegación izquierda para darles igual importancia.
- Desea conservar como contenido mucho espacio como sea posible en los tamaños de ventana pequeña.

A continuación mostramos un ejemplo:

![GIF comportamiento adaptable de panel de navegación superior o izquierdo 1](images/navigation-top-to-left.png)

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

En ocasiones, deben aplicaciones enlazar datos diferentes en el panel superior y el panel izquierdo. A menudo, el panel izquierdo incluye más elementos de navegación.

A continuación mostramos un ejemplo:

![GIF comportamiento adaptable de panel de navegación superior o izquierdo 2](images/navigation-top-to-left2.png)

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

### <a name="tabs"></a>Fichas

En el modelo de fichas, selección y el foco están vinculados. Una acción que normalmente turnos sería también pasar el foco selección. En el debajo de ejemplo, presionando derecha mueve el indicador de selección de presentación hasta Ampliador. Para realizar esta acción estableciendo la propiedad [SelectionFollowsFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.selectionfollowsfocus) en habilitado.

![captura de pantalla de sólo texto navview superior](images/nav-tabs.png)

Este es el ejemplo XAML para que:

```xaml
<NavigationView PanePosition="Top" SelectionFollowsFocus="Enabled" >
   <NavigationView.MenuItems>
        <NavigationViewItem Content="Display" />
        <NavigationViewItem Content="Magnifier"  />
        <NavigationViewItem Content="Keyboard" />
    </NavigationView.MenuItems>
</NavigationView>

```

Para intercambiar contenido cuando se cambia la selección de la ficha, puede usar el método de [NavigateWithOptions](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.NavigateToType) del marco con FrameNavigationOptions.IsNavigationStackEnabled establecido en False y NavigateOptions.TransitionInfoOverride se establece en el correspondiente lado a lado animación de la diapositiva. Para obtener un ejemplo, vea el [ejemplo de código](#code-example) siguiente.

Si desea cambiar el estilo predeterminado, se puede invalidar la propiedad de [MenuItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.menuitemcontainerstyle) del NavigationView. También puede establecer la propiedad [MenuItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.menuitemtemplate) para especificar una plantilla de datos diferentes.

## <a name="backwards-navigation"></a>Navegación hacia atrás

NavigationView tiene un botón Atrás integrado, que se puede habilitar con las siguientes propiedades:

- [**IsBackButtonVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible) es una enumeración NavigationViewBackButtonVisible y "Auto" de manera predeterminada. Se utiliza para mostrar u ocultar el botón Atrás. Cuando el botón no está visible, se contraerá el espacio para dibujar el botón Atrás.
- [**IsBackEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled) es falso de manera predeterminada y puede usarse para alternar los estados del botón Atrás.
- [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) se activa cuando un usuario hace clic en el botón Atrás.
    - En el modo mínimo/compacto, cuando NavigationView.Pane está abierto como un control flotante, al hacer clic en el botón Atrás se cerrará el panel y se activará en su lugar el evento **PaneClosing**.
    - No se activará si IsBackEnabled es falso.

:::row:::
    :::column:::
    <b>Panel de navegación izquierdo</b><br>
    ![NavigationView el botón Atrás en panel de navegación izquierdo](images/leftnav-back.png)
    :::column-end:::
    :::column:::
     <b>Panel de navegación superior</b><br>
    ![NavigationView el botón Atrás en el panel de navegación superior](images/topnav-back.png)
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>Ejemplo de código

> [!NOTE]
> NavigationView debe servir como el contenedor raíz de la aplicación, ya que este control está diseñado para ocupar todo el ancho y el alto de la ventana de la aplicación.
Puedes invalidar los anchos en los que la vista de navegación cambia los modos de visualización usando las propiedades [CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.CompactModeThresholdWidth) y [ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ExpandedModeThresholdWidth).

El siguiente es un ejemplo de end-to-end de cómo se pueden incorporar NavigationView con un panel de navegación superior en tamaños de ventana grandes y un panel de navegación izquierdo en tamaños de ventana pequeña.

En este ejemplo, esperamos que a los usuarios finales con frecuencia seleccione nuevas categorías de navegación y por lo tanto se:

- Establecer la propiedad [SelectionFollowsFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PanePostion) en habilitado
- Navegación de marco de uso que no se agrega a la pila de navegación.
- Mantenga el valor predeterminado en la propiedad [ShoulderNavigationEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PanePostion) , que se usa para indicar si reboteadores izquierda/derecha en un controlador para juegos desplácese por las categorías de navegación de nivel superior de la aplicación. El valor predeterminado es "WhenSelectionFollowsFocus". Los otros valores posibles son "Siempre" y "Nunca".

También se explica cómo implementar la navegación con el botón Atrás del NavigationView con versiones anteriores.

Esta es una grabación de lo que muestra el ejemplo:

![Ejemplo de NavigationView End-To-End](images/nav-code-example.gif)

Este es el código de ejemplo:

> [!NOTE]
> Si está utilizando la [Biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/), a continuación, debe agregar una referencia para el Kit de herramientas: `xmlns:controls="using:Microsoft.UI.Xaml.Controls"`.

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
> Si está utilizando la [Biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/), a continuación, debe agregar una referencia para el Kit de herramientas: `using MUXC = Microsoft.UI.Xaml.Controls;`.

```csharp
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
    ContentFrame.Navigate(item.Page);
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

Fondo del panel muestra en aplicación acrílico cuando NavigationView se encuentra en la parte superior, mínima, o el modo compacto. Para actualizar este comportamiento o personalizar la apariencia del acrílico del panel, modifica los dos recursos de tema sobrescribiéndolos en tu App.xaml.

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

Para un aspecto transparente + haremos, si la aplicación tiene las páginas que usan un ScrollViewer y el panel de navegación es superior situado, se recomienda tener el desplazamiento de contenido en el panel de navegación superior.

Esto se puede lograr estableciendo la propiedad [CanContentRenderOutsideBounds](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.cancontentrenderoutsidebounds) en el ScrollViewer relevante en true.

![navview desplazamiento navpane](images/nav-scroll-content.png)

Si la aplicación tiene mucho tiempo desplazamiento contenido, desea considerar la incorporación de encabezados rápidos que adjuntar al panel de navegación superior y forman una superficie suave. 

![encabezado de navview desplazamiento rápidas](images/nav-scroll-stickyheader.png)

Para realizar esta acción estableciendo la propiedad [ContentOverlay](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ContentOverlay) en NavigationView. 

A veces, si el usuario se desplaza hacia abajo, es posible que desee ocultar el panel de navegación, lograr estableciendo la propiedad [IsPaneVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ContentOverlay) en NavigationView en false.

![panel de navegación de navview desplazamiento ocultar](images/nav-scroll-hidepane.png)

## <a name="related-topics"></a>Temas relacionados

- [Clase NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [Maestro/detalles](master-details.md)
- [Control dinámico](tabs-pivot.md)
- [Conceptos básicos de navegación](../basics/navigation-basics.md)
- [Introducción a Fluent Design para UWP](../fluent-design-system/index.md)