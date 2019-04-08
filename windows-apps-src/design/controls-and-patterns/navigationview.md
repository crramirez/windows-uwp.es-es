---
Description: Control NavigationView es un control adaptable que implementa patrones de navegación de nivel superior de la aplicación.
title: Vista de navegación
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4ba3a45701d82ad0b43591469bf390190ec18db0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642230"
---
# <a name="navigation-view"></a>Vista de navegación

El control NavigationView proporciona navegación de nivel superior de la aplicación. Se adapta a una variedad de tamaños de pantalla y admite tanto _superior_ y _izquierdo_ estilos de navegación.

![navegación superior](images/nav-view-header.png)<br/>
_Admite la vista de navegación superior y el panel de navegación izquierdo o menú_

> **API de plataforma**: [Clase Windows.UI.Xaml.Controls.NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **API de biblioteca de interfaz de usuario de Windows**: [Clase Microsoft.UI.Xaml.Controls.NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> Algunas características del control NavigationView, tales como _superior_ panel de navegación, requiere Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o posterior, o el [biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Control NavigationView es un control de navegación adaptable que funciona bien para:

- Proporcionar una experiencia de navegación coherente en toda la aplicación.
- Conservar espacio en pantalla en ventanas más pequeñas.
- Organizar el acceso a muchas categorías de exploración.

Para otros patrones de navegación, consulte [conceptos básicos del diseño de navegación](../basics/navigation-basics.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/NavigationView">abrir la aplicación y ver NavigationView en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>Modos de presentación

> La propiedad PaneDisplayMode requiere Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o posterior, o el [biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Puede usar la propiedad PaneDisplayMode para configurar los estilos de navegación diferente o modos de visualización, para el control NavigationView.

:::row:::
    :::column:::
    ### <a name="top"></a>Superior
    El panel se coloca sobre el contenido.</br>
    `PaneDisplayMode="Top"`
    :::column-end:::
    :::column span="2":::
    ![Ejemplo de navegación superior](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

Se recomienda _superior_ navegación cuando:

- Tiene 5 o menos categorías de exploración de nivel superior que son igualmente importante y cualquier navegación de nivel superior adicionales categorías que terminan en el menú desplegable de desbordamiento se consideran menos importantes.
- Debe mostrar todas las opciones de navegación en pantalla.
- Quiere tener más espacio para el contenido de la aplicación.
- Los iconos no pueden describir claramente las categorías de navegación de la aplicación.

:::row:::
    :::column:::
    ### <a name="left"></a>Izquierda
    El panel se expande y se coloca a la izquierda del contenido.</br>
    `PaneDisplayMode="Left"`
    :::column-end:::
    :::column span="2":::
    ![Ejemplo de panel de navegación izquierdo expandido](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

Se recomienda _izquierdo_ navegación cuando:

- Tener entre 5 y 10 categorías de exploración de nivel superior igualmente importante.
- Desea que las categorías de exploración a ser muy importantes, con menos espacio para el contenido de la aplicación.

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    El panel muestra los iconos solo hasta que se abre y se coloca a la izquierda del contenido.</br>
    `PaneDisplayMode="LeftCompact"`
    :::column-end:::
    :::column span="2":::
    ![Ejemplo de panel de navegación izquierdo compact](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    Se muestra solo el botón de menú hasta que se abre el panel. Cuando se abre, se coloca a la izquierda del contenido.</br>
    `PaneDisplayMode="LeftMinimal"`
    :::column-end:::
    :::column span="2":::
    ![Ejemplo de panel de navegación izquierdo mínima](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>Automático

De forma predeterminada, PaneDisplayMode se establece en Auto. En el modo Auto, la vista de navegación se adapta entre LeftMinimal cuando la ventana se estrecha a LeftCompact y, a continuación, se deja tal como la ventana obtiene más amplia. Para obtener más información, consulte el [comportamiento adaptable](#adaptive-behavior) sección.

![Comportamiento de navegación izquierdo predeterminado adaptable](images/displaymode-auto.png)<br/>
_Comportamiento adaptable de navegación vista predeterminada_

## <a name="anatomy"></a>Anatomía

Estas imágenes muestra el diseño del panel, encabezado y áreas de contenido del control cuando se configura para _superior_ o _izquierdo_ navegación.

![Diseño de la vista de navegación superior](images/topnav-anatomy.png)<br/>
_Diseño de navegación superior_

![Diseño de la vista de navegación izquierdo](images/leftnav-anatomy.png)<br/>
_Diseño de navegación izquierdo_

### <a name="pane"></a>Panel

Puede usar la propiedad PaneDisplayMode para colocar el panel por encima del contenido o a la izquierda del contenido.

El panel de control NavigationView puede contener:

- [Elemento NavigationViewItem](/uwp/api/windows.ui.xaml.controls.navigationviewitem) objetos. Elementos de navegación para navegar por las páginas específicas.
- [NavigationViewItemSeparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator) objetos. Separadores para agrupar elementos de navegación. Establecer el [opacidad](/uwp/api/windows.ui.xaml.uielement.opacity) propiedad en 0 para representar el separador de espacio.
- [NavigationViewItemHeader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader) objetos. Encabezados para etiquetar los grupos de elementos.
- Opcional [AutoSuggestBox](auto-suggest-box.md) control para permitir la búsqueda de nivel de aplicación. Asigne el control a la [NavigationView.AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.navigationview.autosuggestbox) propiedad.
- Un punto de entrada opcional para [configuración de la aplicación](../app-settings/app-settings-and-data.md). Para ocultar el elemento de configuración, establezca el [IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) propiedad **false**.

También contiene el panel izquierdo:

- Un botón de menú para alternar el panel abierto y cerrado. En ventanas de la aplicación mayores, cuando el panel está abierto, puedes ocultar este botón usando la propiedad [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible).

La vista de navegación tiene un botón Atrás situado en la esquina superior izquierda del panel. Sin embargo, no automáticamente controlar la navegación hacia atrás y agregar contenido a la pila de retroceso. Para habilitar la navegación hacia atrás, consulte el [hacia atrás navegación](#backwards-navigation) sección.

Aquí está la Anatomía de panel detallado para las posiciones del panel izquierdo y superior.

#### <a name="top-navigation-pane"></a>Panel de navegación superior

![Anatomía de panel superior de la vista de navegación](images/navview-pane-anatomy-horizontal.png)

1. Encabezados
1. Elementos de navegación
1. Separadores
1. AutoSuggestBox (opcional)
1. Botón de configuración (opcional)

#### <a name="left-navigation-pane"></a>Panel de navegación izquierdo

![Vista de navegación izquierda Anatomía del panel](images/navview-pane-anatomy-vertical.png)

1. Botón de menú
1. Elementos de navegación
1. Separadores
1. Encabezados
1. AutoSuggestBox (opcional)
1. Botón de configuración (opcional)

#### <a name="pane-footer"></a>Pie de página de panel

Puede colocar contenido en el pie de página del panel libre agregándolo a la [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter) propiedad.

:::row:::
    :::column:::
    ![Barra de navegación superior del pie de página de panel](images/navview-freeform-footer-top.png)<br>
     _Pie de página del panel superior_<br>
    :::column-end:::
    :::column:::
    ![Barra de navegación izquierda del pie de página de panel](images/navview-freeform-footer-left.png)<br>
    _Pie de página del panel izquierdo_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-title-and-header"></a>Encabezado y el título de panel

Puede colocar el contenido de texto en el área de encabezado de panel estableciendo el [PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle) propiedad. Toma una cadena y se muestra el texto situado junto al botón de menú.

Para agregar contenido que no son texto, como una imagen o logotipo, puede colocar cualquier elemento en el encabezado del panel agregándolo a la [PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader) propiedad.

Si se establecen PaneTitle y PaneHeader, el contenido se apila horizontalmente junto al botón de menú, con la más cercana al botón de menú PaneTitle.

:::row:::
    :::column:::
    ![Barra de navegación superior del encabezado de panel](images/navview-freeform-header-top.png)<br>
     _Encabezado del panel superior_<br>
    :::column-end:::
    :::column:::
    ![Barra de navegación izquierda del encabezado de panel](images/navview-freeform-header-left.png)<br>
    _Encabezado del panel izquierdo_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-content"></a>Contenido del panel

Puede colocar contenido en el panel de formato libre agregándolo a la [PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent) propiedad.

:::row:::
    :::column:::
    ![Panel personalizado contenido superior nav](images/navview-freeform-pane-top.png)<br>
     _Contenido personalizado del panel superior_<br>
    :::column-end:::
    :::column:::
    ![Contenido personalizado del panel barra de navegación izquierda](images/navview-freeform-pane-left.png)<br>
    _Contenido personalizado del panel izquierdo_<br>
    :::column-end:::
:::row-end:::

### <a name="header"></a>Encabezado

Puede agregar un título de página estableciendo la [encabezado](/uwp/api/windows.ui.xaml.controls.navigationview.header) propiedad.

![Ejemplo de área de encabezado de la vista de navegación](images/nav-header.png)<br/>
_Encabezado de la vista de navegación_

El área de encabezado se alinea verticalmente con el botón de navegación en la posición del panel izquierdo y se encuentra debajo del panel en la posición del panel superior. Tiene una altura fija de 52 px. Su finalidad es contener el título de la página de la categoría de navegación seleccionada. El encabezado se acopla a la parte superior de la página y actúa como punto de recorte de desplazamiento para el área de contenido.

El encabezado está visible en cualquier momento que el control NavigationView está en modo de pantalla mínima. Puedes elegir ocultar el encabezado de otros modos, que se usan en los anchos de ventana mayores. Para ocultar el encabezado, establezca el [AlwaysShowHeader](/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) propiedad **false**.

### <a name="content"></a>Contenido

![Ejemplo de área de contenido de la vista de navegación](images/nav-content.png)<br/>
_Ver el contenido de navegación_

El área de contenido es donde se muestra la mayor parte de la información para la categoría de navegación seleccionada.

Se recomienda 12 px márgenes para el área de contenido al control NavigationView se encuentra en **mínimo** modo y 24 px márgenes en caso contrario.

## <a name="adaptive-behavior"></a>Comportamiento adaptable

De forma predeterminada, la vista de navegación cambia automáticamente su modo de presentación según la cantidad de espacio de pantalla a su disposición. El [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) y [ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth) propiedades especifican los puntos de interrupción en el que se cambia el modo de presentación. Puede modificar estos valores para personalizar el comportamiento del modo de pantalla adaptable.

### <a name="default"></a>Predeterminado

Cuando se establece PaneDisplayMode en su valor predeterminado de **automática**, es mostrar el comportamiento adaptable:

- Un panel izquierdo expandido en el ancho de ventana grande (1008px o superior).
- A izquierda solo icono Panel de navegación (LeftCompact) en el ancho de la ventana mediana (641px a 1007px).
- Solo un botón de menú (LeftMinimal) de los anchos de la ventana pequeña (640 px o menos).

Para obtener más información acerca de los tamaños de ventana para el comportamiento adaptable, vea [puntos de interrupción y tamaños de pantalla](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

![Comportamiento de navegación izquierdo predeterminado adaptable](images/displaymode-auto.png)<br/>
_Comportamiento adaptable de navegación vista predeterminada_

### <a name="minimal"></a>Mínimo

Un segundo modelo adaptable común es usar un panel izquierdo expandido en el ancho de ventana grande y un botón de menú en ambos anchos de ventanas pequeñas y medianas.

Se recomienda esto cuando:

- Quiere tener más espacio para el contenido de la aplicación en los anchos de ventana más pequeños.
- Las categorías de exploración no se puede representar claramente con iconos.

![Comportamiento adaptable mínima de navegación izquierdo](images/adaptive-behavior-minimal.png)<br/>
_Comportamiento de adaptable "mínimo" de la vista de navegación_

Para configurar este comportamiento, establezca CompactModeThresholdWidth el ancho al que desea que el panel para contraer. En este caso, se cambia el valor predeterminado de 640 a 1007. También debe establecer ExpandedModeThresholdWidth para asegurarse de que los valores no están en conflicto.

```xaml
<NavigationView CompactModeThresholdWidth="1007" ExpandedModeThresholdWidth="1007"/>
```

### <a name="compact"></a>Compacto

Un tercer patrón adaptable común es usar un panel izquierdo expandido en el ancho de ventana grande y un LeftCompact, solo icono Panel de navegación en ambos anchos de ventanas pequeñas y medianas.

Se recomienda esto cuando:

- Es importante mostrar siempre todas las opciones de navegación de pantalla.
- Las categorías de navegación se pueden representar claramente con iconos.

![Comportamiento adaptable compact de navegación izquierdo](images/adaptive-behavior-compact.png)<br/>
_Comportamiento adaptable de "compactar" de la vista de navegación_

Para configurar este comportamiento, establezca CompactModeThresholdWidth en 0.

```xaml
<NavigationView CompactModeThresholdWidth="0"/>
```

### <a name="no-adaptive-behavior"></a>Ningún comportamiento adaptable

Para deshabilitar el comportamiento automático adaptable, establezca PaneDisplayMode en un valor distinto de automático. En este caso, se establece a LeftMinimal, por lo que solo el botón de menú se muestra independientemente del ancho de la ventana.

![Barra de navegación no izquierda ningún comportamiento adaptable](images/adaptive-behavior-none.png)<br/>
_Vista de navegación con PaneDisplayMode establecido en LeftMinimal_

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

Como se describió anteriormente en el _modos de presentación_ sección, puede establecer el panel para estar siempre en la parte superior, siempre expandido, siempre compact o siempre mínima. También puede administrar los modos de presentación en el código de aplicación. En la sección siguiente se muestra un ejemplo de esto.

### <a name="top-to-left-navigation"></a>Arriba a la izquierda

Cuando se utiliza la navegación superior en la aplicación, los elementos de navegación se contraen en un menú de desbordamiento como las salidas de ancho de la ventana. Cuando la ventana de la aplicación es reducida, lo puede proporcionar una mejor experiencia de usuario para cambiar la PaneDisplayMode de arriba a LeftMinimal navegación, en lugar de permitir que todos los elementos se contraen en el menú de desbordamiento.

Se recomienda usar la navegación superior en tamaños de ventana grandes y de navegación izquierdo de pequeño tamaños de intervalo cuando:

- Tiene un conjunto de categorías de navegación de primer nivel importante igualmente mostrar juntos, tal que si una categoría de este conjunto no cabe en la pantalla, contraer para darles importancia igual a la izquierda.
- Desea conservar como contenido mucho espacio como sea posible en tamaños de ventana pequeña.

En este ejemplo se muestra cómo usar un [VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager) y [AdaptiveTrigger.MinWindowWidth](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) propiedad para cambiar entre la parte superior y LeftMinimal navegación.

![Ejemplo de comportamiento adaptable superior o izquierdo 1](images/navigation-top-to-left.png)

```xaml
<Grid >
    <NavigationView x:Name="NavigationViewControl" >
        <NavigationView.MenuItems>
            <NavigationViewItem Content="A" x:Name="A" />
            <NavigationViewItem Content="B" x:Name="B" />
            <NavigationViewItem Content="C" x:Name="C" />
        </NavigationView.MenuItems>
    </NavigationView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger
                        MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                </VisualState.StateTriggers>

                <VisualState.Setters>
                    <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>

```

> [!TIP]
> Cuando usas AdaptiveTrigger.MinWindowWidth, el estado visual se desencadena cuando la ventana es mayor que el ancho mínimo especificado. Esto significa que el valor predeterminado XAML define la estrecha ventana, y VisualState define las modificaciones que se aplican cuando la ventana obtiene más amplia. El valor predeterminado PaneDisplayMode para la vista de navegación es automática, así que cuando el ancho de la ventana es menor o igual que CompactModeThresholdWidth, navegación LeftMinimal se utiliza. Cuando la ventana obtiene más amplia, VisualState invalida el valor predeterminado y se utiliza la navegación superior.

## <a name="navigation"></a>Navegación

La vista de navegación no realiza automáticamente las tareas de exploración. Cuando el usuario puntea un elemento de navegación, la vista de navegación muestra ese elemento seleccionado y genera un [ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked) eventos. Si se produce un nuevo elemento se selecciona, la derivación de un [SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged) también se genera el evento.

Puede controlar cualquier evento para realizar tareas relacionadas con el panel de navegación solicitada. Debe controlar que depende el comportamiento que desee para la aplicación. Normalmente, vaya a la página solicitada y actualizar el encabezado de la vista de navegación en respuesta a estos eventos.

**ItemInvoked** se produce siempre que el usuario puntea un elemento de navegación, incluso si ya está seleccionada. (También se puede invocar el elemento con una acción equivalente con el mouse, teclado u otra entrada. Para obtener más información, consulte [entrada e interacciones](../input/index.md).) Si se desplaza en el controlador de ItemInvoked, de forma predeterminada, se volverá a cargar la página y se agrega una entrada duplicada para la pila de navegación. Si se navega cuando se invoca un elemento, debe impedir a recargar la página, o asegúrese de que no se crea una entrada duplicada en la navegación backstack cuando se vuelve a cargar la página. (Vea los ejemplos de código).

**Evento SelectionChanged** puede generarse por un usuario invoca un elemento que no está seleccionado actualmente o cambiar mediante programación el elemento seleccionado. Si se produce el cambio de selección porque un usuario llamado a un elemento, se produce en primer lugar el evento ItemInvoked. Si el cambio de selección mediante programación, no se genera ItemInvoked.

### <a name="backwards-navigation"></a>Navegación hacia atrás

Control NavigationView presenta un botón Atrás integrado; pero, al igual que con la navegación hacia adelante, no realizar hacia atrás navegación automáticamente. Cuando el usuario pulsa el botón Atrás, la [BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) provoca el evento. Controle este evento para realizar la navegación. Para obtener más información y ejemplos de código, vea [historial de navegación hacia atrás o navegación](../basics/navigation-history-and-backwards-navigation.md).

En el modo compacto o como mínimo, la vista panel de navegación está abierta como un control flotante. En este caso, al hacer clic en el botón Atrás cerrará el panel y generar el **PaneClosing** eventos en su lugar.

Puede ocultar o deshabilitar el botón Atrás al establecer estas propiedades:

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible): usar para mostrar y ocultar el botón Atrás. Esta propiedad toma un valor de la [NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible) enumeración y se establece en **automática** de forma predeterminada. Cuando se contrae el botón, no se reserva ningún espacio para él en el diseño.
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled): usar para habilitar o deshabilitar el botón Atrás. Puede enlazar los datos de esta propiedad en el [CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback) propiedad de su marco de navegación. **BackRequested** no se desencadena si **IsBackEnabled** es **false**.

:::row:::
    :::column:::
        ![Navigation view back button in the left navigation pane](images/leftnav-back.png)<br/>
        _The back button in the left navigation pane_
    :::column-end:::
    :::column:::
        ![Navigation view back button in the top navigation pane](images/topnav-back.png)<br/>
        _The back button in the top navigation pane_
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>Ejemplo de código

En este ejemplo se muestra cómo puede usar el control NavigationView con un panel de navegación superior en tamaños de ventana grande y un panel de navegación izquierdo en tamaños de ventana pequeña. Puede adaptarse a la navegación izquierda quitando los _superior_ configuración de exploración en VisualStateManager.

En el ejemplo se muestra una forma recomendada para configurar los datos de navegación que funcionarán para muchos escenarios comunes. También se muestra cómo implementar la navegación con la navegación hacia atrás de botón y el teclado del control NavigationView con versiones anteriores.

Este código supone que la aplicación contiene páginas con los nombres siguientes para navegar a: _Página principal de_, _AppsPage_, _GamesPage_, _MusicPage_, _MyContentPage_, y _SettingsPage_ . No se muestra el código para estas páginas.

> [!IMPORTANT]
> Información acerca de las páginas de la aplicación se almacena en un [ValueTuple](https://docs.microsoft.com/dotnet/api/system.valuetuple). Esta estructura requiere que la versión mínima para el proyecto de aplicación debe ser SDK 17763 o superior. Si usa la versión WinUI del control NavigationView para tener como destino versiones anteriores de Windows 10, puede usar el [paquete System.ValueTuple NuGet](https://www.nuget.org/packages/System.ValueTuple/) en su lugar.

> [!IMPORTANT]
> Este código muestra cómo usar el [biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/) versión del control NavigationView. Si usa la versión de la plataforma de control NavigationView en su lugar, la versión mínima para el proyecto de aplicación debe ser SDK 17763 o superior. Para usar la versión de la plataforma, quite todas las referencias a `muxc:`.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<Grid>
    <muxc:NavigationView x:Name="NavView"
                         Loaded="NavView_Loaded"
                         ItemInvoked="NavView_ItemInvoked"
                         BackRequested="NavView_BackRequested">
        <muxc:NavigationView.MenuItems>
            <muxc:NavigationViewItem Tag="home" Icon="Home" Content="Home"/>
            <muxc:NavigationViewItemSeparator/>
            <muxc:NavigationViewItemHeader x:Name="MainPagesHeader"
                                           Content="Main pages"/>
            <muxc:NavigationViewItem Tag="apps" Content="Apps">
                <muxc:NavigationViewItem.Icon>
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xEB3C;"/>
                </muxc:NavigationViewItem.Icon>
            </muxc:NavigationViewItem>
            <muxc:NavigationViewItem Tag="games" Content="Games">
                <muxc:NavigationViewItem.Icon>
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE7FC;"/>
                </muxc:NavigationViewItem.Icon>
            </muxc:NavigationViewItem>
            <muxc:NavigationViewItem Tag="music" Icon="Audio" Content="Music"/>
        </muxc:NavigationView.MenuItems>

        <muxc:NavigationView.AutoSuggestBox>
            <!-- See AutoSuggestBox documentation for
                 more info about how to implement search. -->
            <AutoSuggestBox x:Name="NavViewSearchBox" QueryIcon="Find"/>
        </muxc:NavigationView.AutoSuggestBox>

        <ScrollViewer>
            <Frame x:Name="ContentFrame" Padding="12,0,12,24" IsTabStop="True"
                   NavigationFailed="ContentFrame_NavigationFailed"/>
        </ScrollViewer>
    </muxc:NavigationView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger
                        MinWindowWidth="{x:Bind NavView.CompactModeThresholdWidth}"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <!-- Remove the next 3 lines for left-only navigation. -->
                    <Setter Target="NavView.PaneDisplayMode" Value="Top"/>
                    <Setter Target="NavViewSearchBox.Width" Value="200"/>
                    <Setter Target="MainPagesHeader.Visibility" Value="Collapsed"/>
                    <!-- Leave the next line for left-only navigation. -->
                    <Setter Target="ContentFrame.Padding" Value="24,0,24,24"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

> [!IMPORTANT]
> Este código muestra cómo usar el [biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/) versión del control NavigationView. Si usa la versión de la plataforma de control NavigationView en su lugar, la versión mínima para el proyecto de aplicación debe ser SDK 17763 o superior. Para usar la versión de la plataforma, quite todas las referencias a `muxc`.

```csharp
// Add "using" for WinUI controls.
// using muxc = Microsoft.UI.Xaml.Controls;

private void ContentFrame_NavigationFailed(object sender, NavigationFailedEventArgs e)
{
    throw new Exception("Failed to load Page " + e.SourcePageType.FullName);
}

// List of ValueTuple holding the Navigation Tag and the relative Navigation Page
private readonly List<(string Tag, Type Page)> _pages = new List<(string Tag, Type Page)>
{
    ("home", typeof(HomePage)),
    ("apps", typeof(AppsPage)),
    ("games", typeof(GamesPage)),
    ("music", typeof(MusicPage)),
};

private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    // You can also add items in code.
    NavView.MenuItems.Add(new muxc.NavigationViewItemSeparator());
    NavView.MenuItems.Add(new muxc.NavigationViewItem
    {
        Content = "My content",
        Icon = new SymbolIcon((Symbol)0xF1AD),
        Tag = "content"
    });
    _pages.Add(("content", typeof(MyContentPage)));

    // Add handler for ContentFrame navigation.
    ContentFrame.Navigated += On_Navigated;

    // NavView doesn't load any page by default, so load home page.
    NavView.SelectedItem = NavView.MenuItems[0];
    // If navigation occurs on SelectionChanged, this isn't needed.
    // Because we use ItemInvoked to navigate, we need to call Navigate
    // here to load the home page.
    NavView_Navigate("home", new EntranceNavigationTransitionInfo());

    // Add keyboard accelerators for backwards navigation.
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

private void NavView_ItemInvoked(muxc.NavigationView sender,
                                 muxc.NavigationViewItemInvokedEventArgs args)
{
    if (args.IsSettingsInvoked == true)
    {
        NavView_Navigate("settings", args.RecommendedNavigationTransitionInfo);
    }
    else if (args.InvokedItemContainer != null)
    {
        var navItemTag = args.InvokedItemContainer.Tag.ToString();
        NavView_Navigate(navItemTag, args.RecommendedNavigationTransitionInfo);
    }
}

<!-- NavView_SelectionChanged is not used in this example, but is shown for completeness.
     You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
     but not both. -->
private void NavView_SelectionChanged(muxc.NavigationView sender,
                                      muxc.NavigationViewSelectionChangedEventArgs args)
{
    if (args.IsSettingsSelected == true)
    {
        NavView_Navigate("settings", args.RecommendedNavigationTransitionInfo);
    }
    else if (args.SelectedItemContainer != null)
    {
        var navItemTag = args.SelectedItemContainer.Tag.ToString();
        NavView_Navigate(navItemTag, args.RecommendedNavigationTransitionInfo);
    }
}

private void NavView_Navigate(string navItemTag, NavigationTransitionInfo transitionInfo)
{
    Type _page = null;
    if (navItemTag == "settings")
    {
        _page = typeof(SettingsPage);
    }
    else
    {
        var item = _pages.FirstOrDefault(p => p.Tag.Equals(navItemTag));
        _page = item.Page;
    }
    // Get the page type before navigation so you can prevent duplicate
    // entries in the backstack.
    var preNavPageType = ContentFrame.CurrentSourcePageType;

    // Only navigate if the selected page isn't currently loaded.
    if (!(_page is null) && !Type.Equals(preNavPageType, _page))
    {
        ContentFrame.Navigate(_page, null, transitionInfo);
    }
}

private void NavView_BackRequested(muxc.NavigationView sender,
                                   muxc.NavigationViewBackRequestedEventArgs args)
{
    On_BackRequested();
}

private void BackInvoked(KeyboardAccelerator sender,
                         KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}

private bool On_BackRequested()
{
    if (!ContentFrame.CanGoBack)
        return false;

    // Don't go back if the nav pane is overlayed.
    if (NavView.IsPaneOpen &&
        (NavView.DisplayMode == muxc.NavigationViewDisplayMode.Compact ||
         NavView.DisplayMode == muxc.NavigationViewDisplayMode.Minimal))
        return false;

    ContentFrame.GoBack();
    return true;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag.
        NavView.SelectedItem = (muxc.NavigationViewItem)NavView.SettingsItem;
        NavView.Header = "Settings";
    }
    else if (ContentFrame.SourcePageType != null)
    {
        var item = _pages.FirstOrDefault(p => p.Page == e.SourcePageType);

        NavView.SelectedItem = NavView.MenuItems
            .OfType<muxc.NavigationViewItem>()
            .First(n => n.Tag.Equals(item.Tag));

        NavView.Header =
            ((muxc.NavigationViewItem)NavView.SelectedItem)?.Content?.ToString();
    }
}
```

A continuación se presenta un [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/index) versión de la **NavView_ItemInvoked** controlador desde la C# ejemplo de código anterior. La técnica en C++ / c++ / WinRT controlador implica almacenar primero (en la etiqueta de la [ **NavigationViewItem**](/uwp/api/windows.ui.xaml.controls.navigationviewitem)) el nombre de tipo completo de la página a la que desea desplazarse. En el controlador, unbox ese valor, se convertirá en un [ **Windows::UI::Xaml::Interop::TypeName** ](/uwp/api/windows.ui.xaml.interop.typename) de objetos y usarla para desplazarse a la página de destino. No es necesario para la variable de asignación denominada `_pages` que ve en el C# ejemplo; y será capaz de crear pruebas unitarias que confirma que los valores dentro de las etiquetas son de un tipo válido. Consulte también [valores escalares de conversión Boxing y unboxing a IInspectable con C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/boxing).

```cppwinrt
void MainPage::NavView_ItemInvoked(Windows::Foundation::IInspectable const & /* sender */, Windows::UI::Xaml::Controls::NavigationViewItemInvokedEventArgs const & args)
{
    if (args.IsSettingsInvoked())
    {
        // Navigate to Settings.
    }
    else if (args.InvokedItemContainer())
    {
        Windows::UI::Xaml::Interop::TypeName pageTypeName;
        pageTypeName.Name = unbox_value<hstring>(args.InvokedItemContainer().Tag());
        pageTypeName.Kind = Windows::UI::Xaml::Interop::TypeKind::Primitive;
        ContentFrame().Navigate(pageTypeName, nullptr);
    }
}
```

## <a name="navigation-view-customization"></a>Personalización de la vista de navegación

### <a name="pane-backgrounds"></a>Fondos de panel

De forma predeterminada, el panel de control NavigationView utiliza un fondo diferente según el modo de presentación:

- el panel es un color gris sólido cuando se expande a la izquierda, en paralelo con el contenido (en modo izquierdo).
- el panel utiliza en la aplicación acrílico cuando abra como superposición sobre contenido (en modo compacto, mínimo o superior).

Para modificar el fondo del panel, puede reemplazar los recursos de tema XAML utilizados para representar el fondo en cada modo. (Esta técnica se usa en lugar de una sola propiedad PaneBackground con el fin de admitir distintos fondos para diferentes modos de presentación).

Esta tabla muestra qué recursos de tema se usan en cada modo de presentación.

| Modo de pantalla | Recursos de tema |
| ------------ | -------------- |
| Izquierda | NavigationViewExpandedPaneBackground |
| LeftCompact<br/>LeftMinimal | NavigationViewDefaultPaneBackground |
| Superior | NavigationViewTopPaneBackground |

En este ejemplo se muestra cómo reemplazar los recursos de tema en App.xaml. Cuando se invalidan los recursos de tema, deben siempre es proporcionar los diccionarios de recursos "Default" y "Contraste alto" como mínimo y los diccionarios para "Claro" u "Oscuro" recursos según sea necesario. Para obtener más información, consulte [ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries).

> [!IMPORTANT]
> Este código muestra cómo usar el [biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/) verzi AcrylicBrush. Si usa la versión de la plataforma de AcrylicBrush en su lugar, la versión mínima para el proyecto de aplicación debe ser SDK 16299 o superior. Para usar la versión de la plataforma, quite todas las referencias a `muxm:`.

```xaml
<Application
    <!-- ... -->
    xmlns:muxm="using:Microsoft.UI.Xaml.Media">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
                <ResourceDictionary>
                    <ResourceDictionary.ThemeDictionaries>
                        <ResourceDictionary x:Key="Default">
                            <!-- The "Default" theme dictionary is used unless a specific
                                 light, dark, or high contrast dictionary is provided. These
                                 resources should be tested with both the light and dark themes,
                                 and specific light or dark resources provided as needed. -->
                            <muxm:AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
                                   BackgroundSource="Backdrop"
                                   TintColor="LightSlateGray"
                                   TintOpacity=".6"/>
                            <muxm:AcrylicBrush x:Key="NavigationViewTopPaneBackground"
                                   BackgroundSource="Backdrop"
                                   TintColor="{ThemeResource SystemAccentColor}"
                                   TintOpacity=".6"/>
                            <LinearGradientBrush x:Key="NavigationViewExpandedPaneBackground"
                                     StartPoint="0.5,0" EndPoint="0.5,1">
                                <GradientStop Color="LightSlateGray" Offset="0.0" />
                                <GradientStop Color="White" Offset="1.0" />
                            </LinearGradientBrush>
                        </ResourceDictionary>
                        <ResourceDictionary x:Key="HighContrast">
                            <!-- Always include a "HighContrast" dictionary when you override
                                 theme resources. This empty dictionary ensures that the 
                                 default high contrast resources are used when the user
                                 turns on high contrast mode. -->
                        </ResourceDictionary>
                    </ResourceDictionary.ThemeDictionaries>
                </ResourceDictionary>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

## <a name="related-topics"></a>Temas relacionados

- [Clase de control NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [Maestro/detalles](master-details.md)
- [Conceptos básicos de navegación](../basics/navigation-basics.md)
- [Diseño Fluent de introducción a UWP](../fluent-design-system/index.md)
