---
author: jwmsft
Description: NavigationView is an adaptive control that implements top-level navigation patterns for your app.
title: Vista de navegación
template: detail.hbs
ms.author: jimwalk
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9eef1616625ca30b1887e7f317c59f7a75abfeea
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2018
ms.locfileid: "6273712"
---
# <a name="navigation-view"></a>Vista de navegación

El control NavigationView proporciona navegación de nivel superior de la aplicación. Se adapta a una variedad de tamaños de pantalla y es compatible con estilos de navegación _superior_ e _izquierda_ .

![navegación superior](images/nav-view-header.png)<br/>
_Admite la vista de navegación superior y el panel de navegación izquierdo o menú_

> **API de la plataforma**: [clase Windows.UI.Xaml.Controls.NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **Las API de biblioteca de la interfaz de usuario de Windows**: [clase Microsoft.UI.Xaml.Controls.NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> Algunas características de NavigationView, como la navegación de la _parte superior_ , requieren Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o posterior, o en la [Biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

NavigationView es un control de navegación adaptable que funciona bien para:

- Proporcionar una experiencia de navegación coherente en toda la aplicación.
- Conservación la superficie de pantalla en ventanas más pequeñas.
- Organizar el acceso a muchas de las categorías de navegación.

Para otros patrones de navegación, consulta los [conceptos básicos del diseño de navegación](../basics/navigation-basics.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/NavigationView">abrir la aplicación y ver NavigationView en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>Modos de visualización

> La propiedad PaneDisplayMode requiere Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o posterior, o en la [Biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Puedes usar la propiedad PaneDisplayMode para configurar los estilos de navegación diferente o modos de visualización, de NavigationView.

:::row:::
    :::column:::
    ### <a name="top"></a>Top
    El panel se coloca encima del contenido.</br>
    `PaneDisplayMode="Top"`
    :::column-end:::
    :::column span="2":::
    ![Ejemplo de navegación superior](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

Te recomendamos la exploración _superior_ cuando:

- Tiene 5 o menos categorías de navegación de nivel superior que tienen la misma importancia y cualquier navegación de nivel superior adicional categorías que terminan en el menú de desbordamiento desplegable se consideran menos importantes.
- Debes mostrar todas las opciones de navegación en pantalla.
- Desea más espacio para el contenido de la aplicación.
- Los iconos no pueden describir claramente categorías de navegación de la aplicación.

:::row:::
    :::column:::
    ### <a name="left"></a>Izquierda
    El panel está expandido y situado a la izquierda del contenido.</br>
    `PaneDisplayMode="Left"`
    :::column-end:::
    :::column span="2":::
    ![Ejemplo de panel de navegación izquierdo expandido](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

Te recomendamos de navegación _izquierdo_ cuando:

- Tienes categorías de navegación de nivel superior igualmente importante de 5-10.
- Quieres categorías de navegación para que sea muy destacados, con menos espacio para otro contenido de la aplicación.

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    El panel muestra solo iconos hasta que se abre y se coloca a la izquierda del contenido.</br>
    `PaneDisplayMode="LeftCompact"`
    :::column-end:::
    :::column span="2":::
    ![Ejemplo de panel de navegación izquierdo compacto](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    Hasta que el panel se abre, se muestra solo el botón de menú. Cuando se abre, está situado a la izquierda del contenido.</br>
    `PaneDisplayMode="LeftMinimal"`
    :::column-end:::
    :::column span="2":::
    ![Ejemplo de panel de navegación izquierdo mínima](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>Automático

De manera predeterminada, PaneDisplayMode se establece en Auto. En modo automático, la vista de navegación se adapta entre LeftMinimal cuando la ventana es estrecha, para LeftCompact y, a continuación, izquierda como de la ventana obtiene más amplia. Para obtener más información, consulta la sección de [comportamiento adaptable](#adaptive-behavior) .

![Comportamiento adaptable predeterminado de navegación izquierdo](images/displaymode-auto.png)<br/>
_Comportamiento adaptable predeterminado vista de navegación_

## <a name="anatomy"></a>Anatomía

Estas imágenes muestran el diseño del panel, encabezado y áreas de contenido del control cuando se configura para la navegación de la _parte superior_ o a la _izquierda_ .

![Diseño de la vista de navegación superior](images/topnav-anatomy.png)<br/>
_Diseño de navegación superior_

![Diseño de la vista de navegación izquierdo](images/leftnav-anatomy.png)<br/>
_Diseño de navegación izquierdo_

### <a name="pane"></a>Panel

Puedes usar la propiedad PaneDisplayMode para colocar el panel encima del contenido o a la izquierda del contenido.

El panel NavigationView puede contener:

- Objetos [NavigationViewItem](/uwp/api/windows.ui.xaml.controls.navigationviewitem) . Elementos de navegación para navegar a páginas específicas.
- Objetos [NavigationViewItemSeparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator) . Separadores para agrupar elementos de navegación. Establece la propiedad [Opacity](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator.opacity) en 0 para representar el separador de espacio.
- Objetos [NavigationViewItemHeader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader) . Encabezados para etiquetar grupos de elementos.
- Un control [AutoSuggestBox](auto-suggest-box.md) opcional para permitir la búsqueda de nivel de la aplicación. Asignar el control a la propiedad [NavigationView.AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.navigationview.autosuggestbox) .
- Un punto de entrada opcional para la [configuración de la aplicación](../app-settings/app-settings-and-data.md). Para ocultar el elemento de configuración, Establece la propiedad [IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) en **false**.

También contiene el panel izquierdo:

- Un botón de menú para activar o desactivar el panel abierto y cerrado. En ventanas de la aplicación mayores, cuando el panel está abierto, puedes ocultar este botón usando la propiedad [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible).

La vista de navegación tiene un botón Atrás en la que se coloca en la esquina superior izquierda del panel. Sin embargo, no automáticamente controlar la navegación hacia atrás y agregar contenido a la pila de retroceso. Para habilitar la navegación hacia atrás, consulta el [hacia atrás navegación](#backwards-navigation) sección.

Esta es la Anatomía de panel detallada de las posiciones de panel izquierdo y superior.

#### <a name="top-navigation-pane"></a>Panel de navegación superior

![Anatomía de panel superior de la vista de navegación](images/navview-pane-anatomy-horizontal.png)

1. Encabezados
1. Elementos de navegación
1. Separadores
1. AutoSuggestBox (opcional)
1. Botón de configuración (opcional)

#### <a name="left-navigation-pane"></a>Panel de navegación izquierdo

![Vista de navegación izquierda Anatomía de panel](images/navview-pane-anatomy-vertical.png)

1. Botón de menú
1. Elementos de navegación
1. Separadores
1. Encabezados
1. AutoSuggestBox (opcional)
1. Botón de configuración (opcional)

#### <a name="pane-footer"></a>El pie de página de panel

Puedes colocar contenido de forma libre en pie de página del panel agregando a la propiedad [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter) .

:::row:::
    :::column:::
    ![Navegación superior de pie de página de panel](images/navview-freeform-footer-top.png)<br>
     _El pie de página de panel superior_<br>
    :::column-end:::
    :::column:::
    ![Menú de navegación izquierdo del pie de página de panel](images/navview-freeform-footer-left.png)<br>
    _Pie de página del panel de la izquierda_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-title-and-header"></a>Encabezado y el título del panel

Puedes colocar contenido de texto en el área de encabezado de panel estableciendo la propiedad [PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle) . Toma una cadena y muestra el texto junto al botón de menú.

Para agregar el contenido que no es de texto, como una imagen o logotipo, puedes colocar cualquier elemento en el encabezado del panel agregando a la propiedad [PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader) .

Si se establecen PaneTitle y PaneHeader, el contenido se apila horizontalmente junto al botón de menú, con el PaneTitle más cercana al botón de menú.

:::row:::
    :::column:::
    ![Navegación superior del encabezado de panel](images/navview-freeform-header-top.png)<br>
     _Encabezado de panel superior_<br>
    :::column-end:::
    :::column:::
    ![Menú de navegación izquierdo del encabezado de panel](images/navview-freeform-header-left.png)<br>
    _Encabezado de panel izquierdo_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-content"></a>Contenido del panel

Puedes colocar contenido de forma libre en el panel agregando a la propiedad [PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent) .

:::row:::
    :::column:::
    ![Panel navegación superior contenido personalizado](images/navview-freeform-pane-top.png)<br>
     _Contenido personalizado de panel superior_<br>
    :::column-end:::
    :::column:::
    ![Contenido de panel personalizado barra de navegación izquierda](images/navview-freeform-pane-left.png)<br>
    _Panel izquierdo de contenido personalizado_<br>
    :::column-end:::
:::row-end:::

### <a name="header"></a>Encabezado

Puedes agregar un título de página estableciendo la propiedad [Header](/uwp/api/windows.ui.xaml.controls.navigationview.header) .

![Ejemplo del área de encabezado de la vista de navegación](images/nav-header.png)<br/>
_Encabezado de la vista de navegación_

El área de encabezado se alinea verticalmente con el botón de navegación en la posición del panel izquierdo y se encuentra debajo del panel en la posición de panel superior. Tiene una altura fija de 52 px. Su finalidad es contener el título de la página de la categoría de navegación seleccionada. El encabezado se acopla a la parte superior de la página y actúa como punto de recorte de desplazamiento para el área de contenido.

El encabezado es visible en cualquier momento que el NavigationView está en modo de presentación mínima. Puedes elegir ocultar el encabezado de otros modos, que se usan en los anchos de ventana mayores. Para ocultar el encabezado, Establece la propiedad [AlwaysShowHeader](/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) en **false**.

### <a name="content"></a>Contenido

![Ejemplo de área de contenido de la vista de navegación](images/nav-content.png)<br/>
_Contenido de la vista de navegación_

El área de contenido es donde se muestra la mayor parte de la información para la categoría de navegación seleccionada.

Te recomendamos márgenes de 12 píxeles para el área de contenido cuando NavigationView se encuentra en el modo **mínimo** y márgenes de 24 píxeles en caso contrario.

## <a name="adaptive-behavior"></a>Comportamiento adaptable

De manera predeterminada, la vista de navegación cambia automáticamente su modo de pantalla en función de la cantidad de espacio en pantalla disponible para ella. Las propiedades [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) y [ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth) especifican los puntos de interrupción en el que se cambia el modo de presentación. Puedes modificar estos valores para personalizar el comportamiento de modo de presentación adaptable.

### <a name="default"></a>Predeterminado

Cuando PaneDisplayMode se establece en su valor predeterminado de **Auto**, el comportamiento adaptable es mostrar:

- Un panel izquierdo expandido en anchos de ventana grande (1008 píxeles o superior).
- A izquierda, solo icono, el panel de navegación (LeftCompact) en los anchos de ventana mediana (641 a 1007 píxeles).
- Solo un botón de menú (LeftMinimal) en los anchos de ventana pequeña (640 píxeles o menos).

Para obtener más información sobre los tamaños de ventana para un comportamiento adaptativo, ver los [tamaños de pantalla y puntos de interrupción](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

![Comportamiento adaptable predeterminado de navegación izquierdo](images/displaymode-auto.png)<br/>
_Comportamiento adaptable predeterminado vista de navegación_

### <a name="minimal"></a>Mínimo

Un patrón común que adaptable segundo es usar un panel izquierdo expandido en anchos de ventana grande y un botón de menú en ambos anchos de ventana pequeñas y medianas.

Te recomendamos esto cuando:

- Desea más espacio para el contenido de la aplicación en los anchos de ventana más pequeños.
- Las categorías de navegación no se puede representar claramente con iconos.

![Comportamiento adaptable mínima de navegación izquierdo](images/adaptive-behavior-minimal.png)<br/>
_Comportamiento adaptable de navegación vista "mínimo"_

Para configurar este comportamiento, Establece CompactModeThresholdWidth en el ancho en el que quieres que el panel para contraer. Aquí, se cambia el valor predeterminado de 640 a 1007. También deberías establecer ExpandedModeThresholdWidth para garantizar que los valores no entran en conflicto.

```xaml
<NavigationView CompactModeThresholdWidth="1007" ExpandedModeThresholdWidth="1007"/>
```

### <a name="compact"></a>Compacto

Un patrón común que adaptable tercero es usar un panel izquierdo expandido en anchos de ventana grande y un LeftCompact, solo icono, el panel de navegación en ambos anchos de ventana pequeñas y medianas.

Te recomendamos esto cuando:

- Es importante para siempre mostrar todas las opciones de navegación en pantalla.
- Las categorías de navegación se pueden representar claramente con iconos.

![Comportamiento adaptable compacto de navegación izquierdo](images/adaptive-behavior-compact.png)<br/>
_Comportamiento adaptable de "compact" de la vista de navegación_

Para configurar este comportamiento, Establece CompactModeThresholdWidth en 0.

```xaml
<NavigationView CompactModeThresholdWidth="0"/>
```

### <a name="no-adaptive-behavior"></a>Ningún comportamiento adaptable

Para deshabilitar el comportamiento adaptable automática, Establece PaneDisplayMode en un valor distinto de automático. Aquí, se establece a LeftMinimal, por lo tanto, solo el botón de menú se muestra independientemente del ancho de ventana.

![Barra de navegación no izquierda ningún comportamiento adaptable](images/adaptive-behavior-none.png)<br/>
_Vista de navegación con PaneDisplayMode establecido en LeftMinimal_

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

Como se describió anteriormente en la sección de _modos de visualización_ , puedes establecer el panel para estar siempre en superior, siempre expandido, siempre compacto o siempre mínima. También puedes administrar los modos de visualización en el código de la aplicación. Se muestra un ejemplo de esto en la siguiente sección.

### <a name="top-to-left-navigation"></a>La parte superior a la izquierda

Cuando usas navegación superior en la aplicación, elementos de navegación se contraen en un menú de desbordamiento como las salidas de ancho de ventana. Cuando la ventana de la aplicación es estrecha, puede proporcionar una mejor experiencia de usuario para cambiar la PaneDisplayMode desde la parte superior LeftMinimal exploración, en lugar de dejar que todos los elementos se contraen en el menú de desbordamiento.

Te recomendamos que uses de navegación superior en la navegación izquierda en pequeñas y tamaños de ventana grandes tamaños de ventana cuando:

- Tienen un conjunto de categorías de navegación de nivel superior importante igualmente mostrarán juntos, si una categoría en este conjunto no cabe en pantalla, contraer a de navegación izquierdo para darles misma importancia.
- Desea conservar como contenido mucho espacio como sea posible en tamaños de ventana pequeña.

En este ejemplo se muestra cómo usar una propiedad [VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager) y [AdaptiveTrigger.MinWindowWidth](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) para cambiar entre la navegación de la parte superior y LeftMinimal.

![Ejemplo de comportamiento adaptable superior o a la izquierda 1](images/navigation-top-to-left.png)

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
> Cuando usas AdaptiveTrigger.MinWindowWidth, se desencadena el estado visual cuando la ventana es mayor que el ancho mínimo especificado. Esto significa que el código XAML predeterminado define la ventana estrecha y el VisualState define las modificaciones que se aplican cuando la ventana obtiene más amplia. El valor predeterminado PaneDisplayMode para la vista de navegación es automática, por lo tanto, cuando el ancho de ventana es menor o igual a CompactModeThresholdWidth, LeftMinimal navegación se usa. Cuando la ventana, aumenta el VisualState reemplaza el valor predeterminado y se usa la navegación superior.

## <a name="navigation"></a>Navegación

La vista de navegación no realiza automáticamente las tareas de navegación. Cuando el usuario pulsa en un elemento de navegación, la vista de navegación muestra ese elemento como seleccionado y genera un evento [ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked) . Si la pulsación da como resultado un nuevo elemento, también se genera un evento [SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged) .

Puedes controlar cualquier evento para realizar tareas relacionadas con la navegación solicitada. Que uno debes controlar depende el comportamiento deseado de la aplicación. Por lo general, puedes navegar a la página solicitada y actualización el encabezado de la vista de navegación en respuesta a estos eventos.

**ItemInvoked** se genera siempre que el usuario pulsa un elemento de navegación, incluso si ya está seleccionado. (También se puede invocar el elemento con una acción equivalente con mouse, teclado u otra entrada. Para obtener más información, consulta [entrada e interacciones](../input/index.md)). Si se navega en el controlador de ItemInvoked, de manera predeterminada, se volverá a cargar la página y se agrega una entrada duplicada a la pila de navegación. Si se navega cuando se invoca un elemento, debe impide la carga de la página o asegurarse de que no se crea una entrada duplicada en la pila de retroceso de navegación cuando se vuelve a cargar la página. (Consulta los ejemplos de código).

**Evento SelectionChanged** se puede provocar un usuario invocar un elemento que no está actualmente seleccionado, o cambiando mediante programación el elemento seleccionado. Si el cambio de selección se produce debido a un usuario invoca un elemento, el evento ItemInvoked se produce en primer lugar. Si el cambio de selección es mediante programación, no se genera ItemInvoked.

### <a name="backwards-navigation"></a>Navegación hacia atrás

NavigationView tiene un botón Atrás integrado; Sin embargo, al igual que con la navegación hacia adelante, no realiza hacia atrás navegación automáticamente. Cuando el usuario pulsa el botón Atrás, se genera el evento [BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) . Controlen este evento para realizar la navegación hacia atrás. Para obtener más información y ejemplos de código, consulta [historial de navegación hacia atrás o navegación](../basics/navigation-history-and-backwards-navigation.md).

En el modo mínimo o compacto, la vista de navegación panel está abierta como un control flotante. En este caso, el haciendo clic en el botón Atrás se cierra el panel y generar el evento de **PaneClosing** en su lugar.

Puedes ocultar o deshabilitar el botón Atrás estableciendo estas propiedades:

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible): usar para mostrar y ocultar el botón Atrás. Esta propiedad toma un valor de la enumeración [NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible) y se establece en **Auto** de forma predeterminada. Cuando se contrae el botón, ningún espacio está reservado para ella en el diseño.
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled): usar para habilitar o deshabilitar el botón Atrás. Puedes enlazar los datos de esta propiedad a la propiedad [CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback) del marco de navegación. **BackRequested** no se genera si **IsBackEnabled** es **Falso**.

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

En este ejemplo se muestra cómo usar NavigationView con un panel de navegación superior a tamaños de ventana grande y un panel de navegación izquierdo de tamaños de ventana pequeña. Se puedan adaptar a la navegación izquierda mediante la eliminación de la configuración de navegación de la _parte superior_ de la VisualStateManager.

En el ejemplo se muestra una forma recomendada para configurar los datos de navegación que funcionan para muchos escenarios comunes. También muestra cómo implementar la navegación con la navegación de teclado y el botón de retroceso de NavigationView hacia atrás.

Este código se da por hecho que contiene las páginas con los siguientes nombres para navegar a la aplicación: _página principal_, _AppsPage_, _GamesPage_, _MusicPage_, _MyContentPage_e _impresorascambiarconfiguración_. No se muestra el código para estas páginas.

> [!IMPORTANT]
> Información acerca de las páginas de la aplicación se almacena en un [ValueTuple](https://docs.microsoft.com/dotnet/api/system.valuetuple). Esta estructura requiere que la versión mínima para el proyecto de aplicación debe ser SDK 17763 o superior. Si usas la versión de WinUI de NavigationView para seleccionar versiones anteriores de Windows 10, puedes usar el [paquete de System.ValueTuple NuGet](https://www.nuget.org/packages/System.ValueTuple/) en su lugar.

> [!IMPORTANT]
> Este código muestra cómo usar la versión de la [Biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/) de NavigationView. Si usas la versión de plataforma de NavigationView en su lugar, la versión mínima para el proyecto de aplicación debe ser SDK 17763 o superior. Para usar la versión de la plataforma, quitar todas las referencias a `muxc:`.

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
> Este código muestra cómo usar la versión de la [Biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/) de NavigationView. Si usas la versión de plataforma de NavigationView en su lugar, la versión mínima para el proyecto de aplicación debe ser SDK 17763 o superior. Para usar la versión de la plataforma, quitar todas las referencias a `muxc`.

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

## <a name="navigation-view-customization"></a>Personalización de la vista de navegación

### <a name="pane-backgrounds"></a>Fondos del panel

De manera predeterminada, el panel de NavigationView usa un fondo diferente según el modo de pantalla:

- el panel es un color sólido gris cuando se expande a la izquierda, en paralelo con el contenido (en modo izquierdo).
- el panel utiliza los acrílico en la aplicación cuando se abre como una superposición sobre el contenido (en modo compacto, mínimo o superior).

Para modificar el fondo del panel, puedes invalidar los recursos de tema XAML que se usa para representar el fondo en cada modo. (Esta técnica se usará en lugar de una sola propiedad de PaneBackground con el fin de admitir diferentes fondos para diferentes modos de presentación).

Esta tabla muestra qué recurso de tema se usa en cada modo de presentación.

| Modo de pantalla | Recurso de tema |
| ------------ | -------------- |
| Izquierda | NavigationViewExpandedPaneBackground |
| LeftCompact<br/>LeftMinimal | NavigationViewDefaultPaneBackground |
| Top | NavigationViewTopPaneBackground |

En este ejemplo se muestra cómo invalidar los recursos de tema en App.xaml. Al invalidar los recursos de tema, debes siempre proporcionar "Default" y "HighContrast" diccionarios de recursos como mínimo y los diccionarios para "Ligera" o "Dark" recursos según sea necesario. Para obtener más información, consulta [ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries).

> [!IMPORTANT]
> Este código muestra cómo usar la versión de la [Biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/) de AcrylicBrush. Si usas la versión de plataforma de AcrylicBrush en su lugar, la versión mínima para el proyecto de aplicación debe ser SDK 16299 o superior. Para usar la versión de la plataforma, quitar todas las referencias a `muxm:`.

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

- [Clase NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [Maestro/detalles](master-details.md)
- [Conceptos básicos de navegación](../basics/navigation-basics.md)
- [Introducción a Fluent Design para UWP](../fluent-design-system/index.md)