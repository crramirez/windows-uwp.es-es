---
Description: NavigationView es un control adaptable que implementa patrones de navegación de nivel superior para la aplicación.
title: Vista de navegación
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5c5a880cc7291e15e71315d4977b6a28f22b2f00
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91749851"
---
# <a name="navigation-view"></a>Vista de navegación

El control NavigationView proporciona navegación de nivel superior para la aplicación. Se adapta a distintos tamaños de pantalla y admite los estilos de navegación _superior_ e _izquierdo_.

![navegación superior](images/nav-view-header.png)<br/>
_La vista de navegación admite el panel de navegación superior e izquierdo o el menú_

**Obtención de la biblioteca de la interfaz de usuario de Windows**

:::row:::
   :::column:::
      ![Logotipo de WinUI](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      El control **NavigationView** se incluye como parte de la biblioteca de interfaz de usuario de Windows, un paquete NuGet que contiene nuevos controles y características de interfaz de usuario destinados a aplicaciones de Windows. Para obtener más información, incluidas instrucciones sobre la instalación, consulta la [introducción a la biblioteca de la interfaz de usuario de Windows](/uwp/toolkits/winui/).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **API de plataforma**: [clase Windows.UI.Xaml.Controls.NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **API de la biblioteca de interfaz de usuario de Windows**: [clase Microsoft.UI.Xaml.Controls.NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> Algunas características de NavigationView, como la navegación _superior_ y _jerárquica_, requieren Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o posterior, o la [biblioteca de interfaz de usuario de Windows](/uwp/toolkits/winui/).

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

NavigationView es un control de navegación adaptable que funciona bien para:

- Proporcionar una experiencia de navegación coherente entre aplicaciones.
- Conservar la superficie de pantalla en ventanas más pequeñas
- Organizar el acceso a muchas categorías de navegación

Para información sobre otros patrones de navegación, consulta [Conceptos básicos del diseño de navegación](../basics/navigation-basics.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon-sm.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/NavigationView">abrirla y ver NavigationView en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>Modos de pantalla

> La propiedad PaneDisplayMode requiere Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o posterior, o la [biblioteca de interfaz de usuario de Windows](/uwp/toolkits/winui/).

Puedes usar la propiedad PaneDisplayMode para configurar diferentes estilos de navegación, o modos de pantalla, para NavigationView.

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

La navegación _superior_ se recomienda en los siguientes casos:

- Tienes cinco o menos categorías de navegación de nivel superior que son igual de importantes, y el resto de estas categorías que acaban en el menú desplegable de desbordamiento se consideran menos importantes.
- Debes mostrar en pantalla todas las opciones de navegación.
- Quieres tener más espacio para el contenido de la aplicación.
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

Se recomienda la navegación _izquierda_ en los siguientes casos:

- Tienes entre 5 y 10 categorías de navegación de nivel superior igual de importantes.
- Quieres que destaquen, y destinar menos espacio a otro contenido de la aplicación.

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    El panel muestra solo los iconos hasta que se abre y se coloca a la izquierda del contenido.</br>
    `PaneDisplayMode="LeftCompact"`
    :::column-end:::
    :::column span="2":::
    ![Ejemplo de panel de navegación izquierdo compacto](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    El botón de menú solo se muestra hasta que se abre el panel. Cuando se abre, se coloca a la izquierda del contenido.</br>
    `PaneDisplayMode="LeftMinimal"`
    :::column-end:::
    :::column span="2":::
    ![Ejemplo de panel de navegación izquierdo mínimo](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>Automático

De forma predeterminada, PaneDisplayMode se establece en Auto. En el modo Auto, la vista de navegación se adapta entre LeftMinimal, cuando la ventana es estrecha, y LeftCompact y, luego, a la izquierda cuando se ensancha la ventana. Para más información, consulte la sección [Comportamiento adaptable](#adaptive-behavior).

![Comportamiento adaptable predeterminado del panel de navegación izquierdo](images/displaymode-auto.png)<br/>
_Comportamiento adaptable predeterminado de la vista de navegación_

## <a name="anatomy"></a>Anatomía

Estas imágenes muestran el diseño del panel, el encabezado y las áreas de contenido del control cuando se configuran para los estilos de navegación _superior_ o _izquierdo_.

![Diseño de la vista de navegación superior](images/topnav-anatomy.png)<br/>
_Diseño de navegación superior_

![Diseño de la vista de navegación izquierda](images/leftnav-anatomy.png)<br/>
_Diseño de navegación izquierda_

### <a name="pane"></a>Panel

Puedes usar la propiedad PaneDisplayMode para colocar el panel encima del contenido o a su izquierda.

El panel NavigationView puede contener:

- Objetos [NavigationViewItem](/uwp/api/windows.ui.xaml.controls.navigationviewitem). Elementos de navegación para ir a páginas concretas.
- Objetos [NavigationViewItemSeparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator). Separadores para agrupar los elementos de navegación. Establece la propiedad [Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) en 0 para representar el separador como espacio.
- Objetos [NavigationViewItemHeader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader). Encabezados para etiquetar grupos de elementos.
- Un control [AutoSuggestBox](auto-suggest-box.md) opcional para permitir la búsqueda en el nivel de aplicación. Asigna al control la propiedad [NavigationView.AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.navigationview.autosuggestbox).
- Un punto de entrada opcional para la [configuración de la aplicación](../app-settings/guidelines-for-app-settings.md). Para ocultar el elemento de configuración, establece la propiedad [IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) en **false**.

El panel izquierdo también contiene:

- Un botón de menú para alternar entre panel abierto y cerrado. En ventanas de aplicación más grandes, cuando el panel está abierto, puedes ocultar este botón mediante la propiedad [IsPaneToggleButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible).

La vista de navegación tiene un botón Atrás situado en la esquina superior izquierda del panel. Sin embargo, no controla automáticamente la navegación hacia atrás y agrega contenido a la pila de retroceso. Para habilitar la navegación hacia atrás, consulta la sección [Navegación hacia atrás](#backwards-navigation).

Esta es la anatomía detallada de las posiciones del panel superior e izquierdo.

#### <a name="top-navigation-pane"></a>Panel de navegación superior

![Anatomía del panel superior de la vista de navegación](images/navview-pane-anatomy-horizontal.png)

1. Encabezados
1. Elementos de navegación
1. Separadores
1. AutoSuggestBox (opcional)
1. Botón de configuración (opcional)

#### <a name="left-navigation-pane"></a>Panel de navegación izquierdo

![Anatomía del panel izquierdo de la vista de navegación](images/navview-pane-anatomy-vertical.png)

1. Botón de menú
1. Elementos de navegación
1. Separadores
1. Encabezados
1. AutoSuggestBox (opcional)
1. Botón de configuración (opcional)

#### <a name="pane-footer"></a>Pie de página del panel

Puedes colocar contenido de formato libre en el pie de página del panel con solo agregarlo a la propiedad [PaneFooter](/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter).

:::row:::
    :::column:::
    ![Pie de página del panel: navegación superior](images/navview-freeform-footer-top.png)<br>
     _Pie de página del panel superior_<br>
    :::column-end:::
    :::column:::
    ![Pie de página del panel: navegación izquierda](images/navview-freeform-footer-left.png)<br>
    _Pie de página del panel izquierdo_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-title-and-header"></a>Encabezado y título del panel

Puedes colocar contenido de texto en el área de encabezado del panel mediante la propiedad [PaneTitle](/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle). Esta propiedad toma una cadena y muestra el texto situado junto al botón de menú.

Para agregar contenido que no sea texto, como imágenes o logotipos, agrega a la propiedad [PaneHeader](/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader) los elementos que quieras colocar en el encabezado del panel.

Si ambas propiedades, PaneTitle y PaneHeader, están establecidas, el contenido se apila horizontalmente junto al botón de menú, con la propiedad PaneTitle más cercana al botón de menú.

:::row:::
    :::column:::
    ![Encabezado del panel: navegación superior](images/navview-freeform-header-top.png)<br>
     _Encabezado del panel superior_<br>
    :::column-end:::
    :::column:::
    ![Encabezado del panel: navegación izquierda](images/navview-freeform-header-left.png)<br>
    _Encabezado del panel izquierdo_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-content"></a>Contenido del panel

Puedes colocar contenido de formato libre en el panel si lo agregas a la propiedad [PaneCustomContent](/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent).

:::row:::
    :::column:::
    ![Contenido personalizado del panel: navegación superior](images/navview-freeform-pane-top.png)<br>
     _Contenido personalizado del panel superior_<br>
    :::column-end:::
    :::column:::
    ![Contenido personalizado del panel: navegación izquierda](images/navview-freeform-pane-left.png)<br>
    _Contenido personalizado del panel izquierdo_<br>
    :::column-end:::
:::row-end:::

### <a name="header"></a>Header

Puedes agregar un título de página mediante la propiedad [Header](/uwp/api/windows.ui.xaml.controls.navigationview.header).

![Ejemplo de área de encabezado de la vista de navegación](images/nav-header.png)<br/>
_Encabezado de la vista de navegación_

El área de encabezado se alinea verticalmente con el botón de navegación en la posición del panel izquierdo y yace debajo del panel en la posición del panel superior. Tiene una altura fija de 52 px. Su finalidad es contener el título de la página de la categoría de navegación seleccionada. El encabezado se acopla a la parte superior de la página y actúa como punto de recorte de desplazamiento para el área de contenido.

El encabezado está visible cada vez que el control NavigationView está en modo de pantalla mínima. Puedes elegir ocultar el encabezado en otros modos, que se usan en anchos de ventana mayores. Para ocultar el encabezado, establece la propiedad [AlwaysShowHeader](/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) en **false**.

### <a name="content"></a>Contenido

![Ejemplo de área de contenido de la vista de navegación](images/nav-content.png)<br/>
_Contenido de la vista de navegación_

El área de contenido es donde se muestra la mayor parte de la información para la categoría de navegación seleccionada.

Te recomendamos márgenes de 12 píxeles en el área de contenido cuando NavigationView se encuentre en el modo **mínimo** y márgenes de 24 píxeles en caso contrario.

## <a name="adaptive-behavior"></a>Comportamiento adaptable

De manera predeterminada, la vista de navegación cambia automáticamente su modo de pantalla en función de la cantidad de espacio en pantalla disponible para ella. Las propiedades [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) y [ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth) especifican los puntos de interrupción en los que el modo de pantalla cambia. Puedes modificar estos valores para personalizar el comportamiento del modo de pantalla adaptable.

### <a name="default"></a>Valor predeterminado

Cuando PaneDisplayMode está establecido en su valor predeterminado de **Auto**, el comportamiento adaptable es mostrar:

- Un panel izquierdo expandido en anchos de ventana grandes (1008 px o mayor).
- Un panel de navegación izquierdo solo con iconos (LeftCompact) en anchos de ventana medios (de 641 px a 1007 px).
- Solo un botón de menú (LeftMinimal) en anchos de ventana pequeños (640 px o menos).

Para más información sobre los tamaños de ventana para el comportamiento adaptable, consulta [Tamaños de pantalla y puntos de interrupción](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

![Comportamiento adaptable predeterminado del panel de navegación izquierdo](images/displaymode-auto.png)<br/>
_Comportamiento adaptable predeterminado de la vista de navegación_

### <a name="minimal"></a>Mínimo

Un segundo patrón adaptable común es usar un panel izquierdo expandido en anchos de ventana grandes y solo un botón de menú en anchos de ventana pequeños y medianos.

Este patrón se recomienda en los siguientes casos:

- Quieres más espacio para el contenido de la aplicación en anchos de ventana más pequeños.
- Las categorías de navegación no se pueden representar claramente con iconos.

![Comportamiento adaptable mínimo de navegación izquierda](images/adaptive-behavior-minimal.png)<br/>
_Comportamiento adaptable "mínimo" de la vista de navegación_

Para configurar este comportamiento, establece CompactModeThresholdWidth en el ancho en el que quieres que se contraiga el panel. En este caso, se cambia el valor predeterminado de 640 a 1007. También debes establecer ExpandedModeThresholdWidth para asegurarte de que los valores no entran en conflicto.

```xaml
<NavigationView CompactModeThresholdWidth="1007" ExpandedModeThresholdWidth="1007"/>
```

### <a name="compact"></a>Compacto

Un tercer patrón adaptable común es usar un panel izquierdo expandido en anchos de ventana grandes y un panel de navegación LeftCompact solo de iconos en anchos de ventana pequeños y medianos.

Este patrón se recomienda en los siguientes casos:

- Es importante mostrar siempre todas las opciones de navegación en la pantalla.
- Las categorías de navegación se pueden representar claramente con iconos.

![Comportamiento adaptable compacto de navegación izquierda](images/adaptive-behavior-compact.png)<br/>
_Comportamiento adaptable "compacto" de la vista de navegación_

Para configurar este comportamiento, establece CompactModeThresholdWidth en 0.

```xaml
<NavigationView CompactModeThresholdWidth="0"/>
```

### <a name="no-adaptive-behavior"></a>Sin comportamiento adaptable

Para deshabilitar el comportamiento adaptable automático, establece PaneDisplayMode en un valor distinto de Auto. En este caso, se establece en LeftMinimal, así que solo se muestra el botón de menú, con independencia del ancho de ventana.

![Sin comportamiento adaptable de navegación izquierda](images/adaptive-behavior-none.png)<br/>
_Vista de navegación con PaneDisplayMode establecido en LeftMinimal_

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

Como se describió anteriormente en la sección _Modos de pantalla_, puedes configurar el panel para que siempre esté arriba, siempre expandido, siempre compacto o siempre mínimo. También puedes administrar los modos de pantalla por tu cuenta en el código de la aplicación. En la sección siguiente se muestra un ejemplo de esto.

### <a name="top-to-left-navigation"></a>Navegación de arriba hacia la izquierda

Cuando usas la navegación superior en la aplicación, los elementos de navegación se contraen en un menú de desbordamiento a medida que el ancho de ventana disminuye. Cuando la ventana de la aplicación es estrecha, se puede proporcionar una mejor experiencia de usuario si se cambia PaneDisplayMode de la navegación Top a LeftMinimal, en lugar de permitir que todos los elementos se contraigan en el menú de desbordamiento.

Se recomienda usar la navegación superior en tamaños de ventana grandes y la navegación izquierda en tamaños de ventana pequeños en los siguientes casos:

- Tienes un conjunto de categorías de navegación de nivel superior igual de importantes para mostrarse juntas, de forma que si una categoría de este conjunto no cabe en la pantalla, la contraes al panel de navegación izquierdo para darle la misma importancia.
- Deseas conservar tanto espacio de contenido como sea posible en los tamaños de ventana pequeños.

En este ejemplo se muestra cómo usar una propiedad [VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager) y [AdaptiveTrigger.MinWindowWidth](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) para cambiar entre la navegación Top y LeftMinimal.

![Ejemplo de comportamiento adaptable superior o izquierdo 1](images/navigation-top-to-left.png)

```xaml
<Grid>
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
> Cuando usas AdaptiveTrigger.MinWindowWidth, el estado visual se desencadena cuando la ventana es más ancha que el ancho mínimo especificado. Esto significa que el código XAML predeterminado define la ventana estrecha y VisualState define las modificaciones que se aplican cuando la ventana se ensancha. La propiedad PaneDisplayMode predeterminada de la vista de navegación es Auto, así que cuando el ancho de la ventana es menor o igual que CompactModeThresholdWidth, se usa la navegación LeftMinimal. Cuando la ventana se ensancha, VisualState invalida el valor predeterminado y se usa la navegación superior.

## <a name="navigation"></a>Navegación

La vista de navegación no realiza automáticamente ninguna tarea de navegación. Cuando el usuario pulsa en un elemento de navegación, la vista de navegación muestra ese elemento como seleccionado y genera un evento [ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked). Si la pulsación da lugar a la selección de un nuevo elemento, también se genera un evento [SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged).

Puedes controlar cualquier evento para realizar tareas relacionadas con la navegación solicitada. Que debas controlar uno u otro depende del comportamiento que quieras para la aplicación. Normalmente, vas a la página solicitada y actualizas el encabezado de la vista de navegación en respuesta a estos eventos.

**ItemInvoked** se produce siempre que el usuario pulsa en un elemento de navegación, incluso si ya está seleccionado. (El elemento también se puede invocar con una acción equivalente con el mouse, el teclado u otro medio de entrada. Para más información, consulta [Entrada e interacciones](../input/index.md)). Si navegas en el controlador ItemInvoked, la página se vuelve a cargar de forma predeterminada y se agrega una entrada duplicada a la pila de navegación. Si navegas cuando se invoca un elemento, debes impedir que se vuelva a cargar la página, o asegúrate de que no se crea una entrada duplicada en la pila de retroceso de navegación cuando se vuelve a cargar la página. (Mira los ejemplos de código).

**SelectionChanged** puede generarse cuando un usuario invoca un elemento que no está seleccionado actualmente o cambia el elemento seleccionado mediante programación. Si el cambio de selección se produce porque un usuario invocó un elemento, el evento ItemInvoked se produce primero. Si el cambio de selección se realiza mediante programación, no se genera ItemInvoked.

### <a name="backwards-navigation"></a>Navegación hacia atrás

NavigationView presenta un botón Atrás integrado; pero, al igual que con la navegación hacia adelante, no realiza la navegación hacia atrás automáticamente. Cuando el usuario pulsa el botón Atrás, se genera el evento [BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested). Es necesario controlar este evento para realizar la navegación hacia atrás. Para más información y obtener ejemplos de código, consulta [Historial de navegación y navegación hacia atrás](../basics/navigation-history-and-backwards-navigation.md).

En el modo compacto o mínimo, el panel de la vista de navegación está abierto como un control flotante. En este caso, al hacer clic en el botón Atrás se cierra el panel y se genera el evento **PaneClosing**.

Puedes ocultar o deshabilitar el botón Atrás mediante el establecimiento de estas propiedades:

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible): se usa para mostrar y ocultar el botón Atrás. Esta propiedad toma un valor de la enumeración [NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible) y se establece en **Auto** de forma predeterminada. Cuando se contrae el botón, no se reserva espacio para él en el diseño.
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled): se usa para habilitar o deshabilitar el botón Atrás. Puedes enlazar los datos de esta propiedad a la propiedad [CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback) de tu marco de navegación. **BackRequested** no se genera si **IsBackEnabled** es **false**.

:::row:::
    :::column:::
        ![Botón atrás de la vista de navegación en el panel de navegación izquierdo](images/leftnav-back.png)<br/>
        _Botón atrás en el panel de navegación izquierdo_
    :::column-end:::
    :::column:::
        ![Botón atrás de la vista de navegación en el panel de navegación superior](images/topnav-back.png)<br/>
        _Botón atrás en el panel de navegación superior_
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>Ejemplo de código

> [!IMPORTANT]
> Para cualquier proyecto que use el kit de herramientas de la biblioteca de Windows UI (WinUI), deberá realizar los mismos pasos de configuración preliminar. Para más información sobre el contexto, la configuración y la compatibilidad, consulta [Getting started with the Windows UI Library](/uwp/toolkits/winui/getting-started) (Introducción a la biblioteca de interfaz de usuario de Windows).

En este ejemplo se muestra cómo puede usar **NavigationView** con un panel de navegación superior en tamaños de ventana grandes y un panel de navegación izquierdo en tamaños de ventana pequeños. Este control se puede adaptar a solo la navegación izquierda si se quita el valor de navegación *top* en **VisualStateManager**.

En el ejemplo se muestra una manera recomendada de configurar los datos de navegación que funciona en muchos escenarios comunes. También se muestra cómo implementar la navegación hacia atrás con el botón Atrás de **NavigationView** y la navegación con el teclado.

Este código supone que la aplicación contiene páginas con los nombres siguientes para ir a: *HomePage*, *AppsPage*, *GamesPage*, *MusicPage*, *MyContentPage* y *SettingsPage*. No se muestra el código de estas páginas.

> [!IMPORTANT]
> La información sobre las páginas de la aplicación se almacena en una instancia de [ValueTuple](/dotnet/api/system.valuetuple). Esta estructura requiere que la versión mínima del proyecto de aplicación sea SDK 17763 o superior. Si usas la versión WinUI de NavigationView para dirigirte a versiones anteriores de Windows 10, puedes usar el [paquete System.ValueTuple NuGet](https://www.nuget.org/packages/System.ValueTuple/) en su lugar.

> [!IMPORTANT]
> Este código muestra cómo usar la versión de la [biblioteca de interfaz de usuario de Windows](/uwp/toolkits/winui/) de NavigationView. Si usas en cambio la versión de la plataforma de NavigationView, la versión mínima del proyecto de aplicación debe ser SDK 17763 o superior. Para usar la versión de la plataforma, quita todas las referencias a `muxc:`.

```xaml
<Page ... xmlns:muxc="using:Microsoft.UI.Xaml.Controls" ... >
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
                        MinWindowWidth="{x:Bind NavViewCompactModeThresholdWidth}"/>
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
</Page>
```

> [!IMPORTANT]
> Este código muestra cómo usar la versión de la [biblioteca de interfaz de usuario de Windows](/uwp/toolkits/winui/) de NavigationView. Si usas en cambio la versión de la plataforma de NavigationView, la versión mínima del proyecto de aplicación debe ser SDK 17763 o superior. Para usar la versión de la plataforma, quita todas las referencias a `muxc`.

```csharp
// Add "using" for WinUI controls.
// using muxc = Microsoft.UI.Xaml.Controls;

private double NavViewCompactModeThresholdWidth { get { return NavView.CompactModeThresholdWidth; } }

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
    NavView_Navigate("home", new Windows.UI.Xaml.Media.Animation.EntranceNavigationTransitionInfo());

    // Add keyboard accelerators for backwards navigation.
    var goBack = new KeyboardAccelerator { Key = Windows.System.VirtualKey.GoBack };
    goBack.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(goBack);

    // ALT routes here
    var altLeft = new KeyboardAccelerator
    {
        Key = Windows.System.VirtualKey.Left,
        Modifiers = Windows.System.VirtualKeyModifiers.Menu
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

// NavView_SelectionChanged is not used in this example, but is shown for completeness.
// You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
// but not both.
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

private void NavView_Navigate(
    string navItemTag,
    Windows.UI.Xaml.Media.Animation.NavigationTransitionInfo transitionInfo)
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

> [!NOTE]
> Para la versión [C++/WinRT](../../cpp-and-winrt-apis/index.md) de este ejemplo de código, empiece creando un nuevo proyecto en la plantilla de proyecto **Aplicación vacía (C++/WinRT)** y, a continuación, agregue el código a la lista de los archivos de código fuente indicados. Para usar el código fuente exactamente como se muestra en la lista, asigne al nuevo proyecto el nombre *NavigationViewCppWinRT*.

```cppwinrt
// MainPage.idl
runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
{
    ...
    Double NavViewCompactModeThresholdWidth{ get; };
}

// pch.h
...
#include "winrt/Windows.UI.Xaml.Input.h"
#include "winrt/Windows.UI.Xaml.Media.Animation.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"

// MainPage.h
#pragma once

#include "MainPage.g.h"

namespace muxc
{
    using namespace winrt::Microsoft::UI::Xaml::Controls;
};

namespace wuxc
{
    using namespace winrt::Windows::UI::Xaml::Controls;
};

namespace winrt::NavigationViewCppWinRT::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        double NavViewCompactModeThresholdWidth();
        void ContentFrame_NavigationFailed(
            Windows::Foundation::IInspectable const& /* sender */,
            Windows::UI::Xaml::Navigation::NavigationFailedEventArgs const& args);
        void NavView_Loaded(
            Windows::Foundation::IInspectable const& /* sender */,
            Windows::UI::Xaml::RoutedEventArgs const& /* args */);
        void NavView_ItemInvoked(
            Windows::Foundation::IInspectable const& /* sender */,
            muxc::NavigationViewItemInvokedEventArgs const& args);

        // NavView_SelectionChanged is not used in this example, but is shown for completeness.
        // You'll typically handle either ItemInvoked or SelectionChanged to perform navigation,
        // but not both.
        void NavView_SelectionChanged(
            muxc::NavigationView const& /* sender */,
            muxc::NavigationViewSelectionChangedEventArgs const& args);
        void NavView_Navigate(
            std::wstring navItemTag,
            Windows::UI::Xaml::Media::Animation::NavigationTransitionInfo const& transitionInfo);
        void NavView_BackRequested(
            muxc::NavigationView const& /* sender */,
            muxc::NavigationViewBackRequestedEventArgs const& /* args */);
        void BackInvoked(
            Windows::UI::Xaml::Input::KeyboardAccelerator const& /* sender */,
            Windows::UI::Xaml::Input::KeyboardAcceleratorInvokedEventArgs const& args);
        bool On_BackRequested();
        void On_Navigated(
            Windows::Foundation::IInspectable const& /* sender */,
            Windows::UI::Xaml::Navigation::NavigationEventArgs const& args);

    private:
        // Vector of std::pair holding the Navigation Tag and the relative Navigation Page.
        std::vector<std::pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>> m_pages;
    };
}

namespace winrt::NavigationViewCppWinRT::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}

// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

namespace winrt::NavigationViewCppWinRT::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();
        m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>
            (L"home", winrt::xaml_typename<NavigationViewCppWinRT::HomePage>()));
        m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>
            (L"apps", winrt::xaml_typename<NavigationViewCppWinRT::AppsPage>()));
        m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>
            (L"games", winrt::xaml_typename<NavigationViewCppWinRT::GamesPage>()));
        m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>
            (L"music", winrt::xaml_typename<NavigationViewCppWinRT::MusicPage>()));
    }

    double MainPage::NavViewCompactModeThresholdWidth()
    {
        return NavView().CompactModeThresholdWidth();
    }

    void MainPage::ContentFrame_NavigationFailed(
        Windows::Foundation::IInspectable const& /* sender */,
        Windows::UI::Xaml::Navigation::NavigationFailedEventArgs const& args)
    {
        throw winrt::hresult_error(
            E_FAIL, winrt::hstring(L"Failed to load Page ") + args.SourcePageType().Name);
    }

    // List of ValueTuple holding the Navigation Tag and the relative Navigation Page
    std::vector<std::pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>> m_pages;

    void MainPage::NavView_Loaded(
        Windows::Foundation::IInspectable const& /* sender */,
        Windows::UI::Xaml::RoutedEventArgs const& /* args */)
    {
        // You can also add items in code.
        NavView().MenuItems().Append(muxc::NavigationViewItemSeparator());
        muxc::NavigationViewItem navigationViewItem;
        navigationViewItem.Content(winrt::box_value(L"My content"));
        navigationViewItem.Icon(wuxc::SymbolIcon(static_cast<wuxc::Symbol>(0xF1AD)));
        navigationViewItem.Tag(winrt::box_value(L"content"));
        NavView().MenuItems().Append(navigationViewItem);
        m_pages.push_back(
            std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>(
                L"content", winrt::xaml_typename<NavigationViewCppWinRT::MyContentPage>()));

        // Add handler for ContentFrame navigation.
        ContentFrame().Navigated({ this, &MainPage::On_Navigated });

        // NavView doesn't load any page by default, so load home page.
        NavView().SelectedItem(NavView().MenuItems().GetAt(0));
        // If navigation occurs on SelectionChanged, then this isn't needed.
        // Because we use ItemInvoked to navigate, we need to call Navigate
        // here to load the home page.
        NavView_Navigate(L"home",
            Windows::UI::Xaml::Media::Animation::EntranceNavigationTransitionInfo());

        // Add keyboard accelerators for backwards navigation.
        Windows::UI::Xaml::Input::KeyboardAccelerator goBack;
        goBack.Key(Windows::System::VirtualKey::GoBack);
        goBack.Invoked({ this, &MainPage::BackInvoked });
        KeyboardAccelerators().Append(goBack);

        // ALT routes here
        Windows::UI::Xaml::Input::KeyboardAccelerator altLeft;
        goBack.Key(Windows::System::VirtualKey::Left);
        goBack.Modifiers(Windows::System::VirtualKeyModifiers::Menu);
        goBack.Invoked({ this, &MainPage::BackInvoked });
        KeyboardAccelerators().Append(altLeft);
    }

    void MainPage::NavView_ItemInvoked(
        Windows::Foundation::IInspectable const& /* sender */,
        muxc::NavigationViewItemInvokedEventArgs const& args)
    {
        if (args.IsSettingsInvoked())
        {
            NavView_Navigate(L"settings", args.RecommendedNavigationTransitionInfo());
        }
        else if (args.InvokedItemContainer())
        {
            NavView_Navigate(
                winrt::unbox_value_or<winrt::hstring>(
                    args.InvokedItemContainer().Tag(), L"").c_str(),
                args.RecommendedNavigationTransitionInfo());
        }
    }

    // NavView_SelectionChanged is not used in this example, but is shown for completeness.
    // You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
    // but not both.
    void MainPage::NavView_SelectionChanged(
        muxc::NavigationView const& /* sender */,
        muxc::NavigationViewSelectionChangedEventArgs const& args)
    {
        if (args.IsSettingsSelected())
        {
            NavView_Navigate(L"settings", args.RecommendedNavigationTransitionInfo());
        }
        else if (args.SelectedItemContainer())
        {
            NavView_Navigate(
                winrt::unbox_value_or<winrt::hstring>(
                    args.SelectedItemContainer().Tag(), L"").c_str(),
                args.RecommendedNavigationTransitionInfo());
        }
    }

    void MainPage::NavView_Navigate(
        std::wstring navItemTag,
        Windows::UI::Xaml::Media::Animation::NavigationTransitionInfo const& transitionInfo)
    {
        Windows::UI::Xaml::Interop::TypeName pageTypeName;
        if (navItemTag == L"settings")
        {
            pageTypeName = winrt::xaml_typename<NavigationViewCppWinRT::SettingsPage>();
        }
        else
        {
            for (auto&& eachPage : m_pages)
            {
                if (eachPage.first == navItemTag)
                {
                    pageTypeName = eachPage.second;
                    break;
                }
            }
        }
        // Get the page type before navigation so you can prevent duplicate
        // entries in the backstack.
        Windows::UI::Xaml::Interop::TypeName preNavPageType =
            ContentFrame().CurrentSourcePageType();

        // Navigate only if the selected page isn't currently loaded.
        if (pageTypeName.Name != L"" && preNavPageType.Name != pageTypeName.Name)
        {
            ContentFrame().Navigate(pageTypeName, nullptr, transitionInfo);
        }
    }

    void MainPage::NavView_BackRequested(
        muxc::NavigationView const& /* sender */,
        muxc::NavigationViewBackRequestedEventArgs const& /* args */)
    {
        On_BackRequested();
    }

    void MainPage::BackInvoked(
        Windows::UI::Xaml::Input::KeyboardAccelerator const& /* sender */,
        Windows::UI::Xaml::Input::KeyboardAcceleratorInvokedEventArgs const& args)
    {
        On_BackRequested();
        args.Handled(true);
    }

    bool MainPage::On_BackRequested()
    {
        if (!ContentFrame().CanGoBack())
            return false;

        // Don't go back if the nav pane is overlaid.
        if (NavView().IsPaneOpen() &&
            (NavView().DisplayMode() == muxc::NavigationViewDisplayMode::Compact ||
                NavView().DisplayMode() == muxc::NavigationViewDisplayMode::Minimal))
            return false;

        ContentFrame().GoBack();
        return true;
    }

    void MainPage::On_Navigated(
        Windows::Foundation::IInspectable const& /* sender */,
        Windows::UI::Xaml::Navigation::NavigationEventArgs const& args)
    {
        NavView().IsBackEnabled(ContentFrame().CanGoBack());

        if (ContentFrame().SourcePageType().Name ==
            winrt::xaml_typename<NavigationViewCppWinRT::SettingsPage>().Name)
        {
            // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag.
            NavView().SelectedItem(NavView().SettingsItem().as<muxc::NavigationViewItem>());
            NavView().Header(winrt::box_value(L"Settings"));
        }
        else if (ContentFrame().SourcePageType().Name != L"")
        {
            for (auto&& eachPage : m_pages)
            {
                if (eachPage.second.Name == args.SourcePageType().Name)
                {
                    for (auto&& eachMenuItem : NavView().MenuItems())
                    {
                        auto navigationViewItem =
                            eachMenuItem.try_as<muxc::NavigationViewItem>();
                        {
                            if (navigationViewItem)
                            {
                                winrt::hstring hstringValue =
                                    winrt::unbox_value_or<winrt::hstring>(
                                        navigationViewItem.Tag(), L"");
                                if (hstringValue == eachPage.first)
                                {
                                    NavView().SelectedItem(navigationViewItem);
                                    NavView().Header(navigationViewItem.Content());
                                }
                            }
                        }
                    }
                    break;
                }
            }
        }
    }
}
```

### <a name="alternative-cwinrt-implementation"></a>Implementación de C++/WinRT alternativa

El código de C# y C++/WinRT mostrado anteriormente se ha diseñado para que pueda usar el mismo marcado XAML para ambas versiones. Sin embargo, hay otra manera de implementar la versión de C++/WinRT que se describe en esta sección, que quizás le resulte más adecuada.

A continuación se muestra una versión alternativa del controlador **NavView_ItemInvoked**. La técnica de esta versión del controlador implica almacenar primero (en la etiqueta de [**NavigationViewItem**](/uwp/api/windows.ui.xaml.controls.navigationviewitem)) el nombre de tipo completo de la página a la que quiere ir. En el controlador, aplica la conversión unboxing a ese valor, conviértelo en un objeto [**Windows::UI::Xaml::Interop::TypeName**](/uwp/api/windows.ui.xaml.interop.typename) y úsalo para ir a la página de destino. No es necesaria la variable de asignación denominada `_pages` que ve en los ejemplos anteriores. Podrá crear pruebas unitarias que confirmen que los valores dentro de las etiquetas son de un tipo válido. Consulta también [Conversión boxing y unboxing de valores escalares a IInspectable con C++/WinRT](../../cpp-and-winrt-apis/boxing.md).

```cppwinrt
void MainPage::NavView_ItemInvoked(
    Windows::Foundation::IInspectable const & /* sender */,
    Windows::UI::Xaml::Controls::NavigationViewItemInvokedEventArgs const & args)
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

## <a name="hierarchical-navigation"></a>Navegación jerárquica
Algunas aplicaciones pueden tener una estructura jerárquica más compleja que requiere más que una lista plana de elementos de navegación. Puede que desees usar elementos de navegación de nivel superior para mostrar categorías de páginas, con elementos secundarios que muestren páginas específicas. También resulta útil si tienes páginas de estilo de concentrador que solo se vinculan a otras páginas. Para estos tipos de casos, debes crear una clase NavigationView jerárquica.

Para mostrar una lista jerárquica de elementos de navegación anidados en el panel, usa la propiedad [MenuItems](/uwp/api/microsoft.ui.xaml.controls.navigationviewitem.menuitems?view=winui-2.4) o la propiedad [MenuItemsSource](/uwp/api/microsoft.ui.xaml.controls.navigationviewitem.menuitemssource?view=winui-2.4) de **NavigationViewItem**.
Cada elemento NavigationViewItem puede contener otros elementos NavigationViewItems, y organizarlos como encabezados y separadores de elementos. Para mostrar una lista jerárquica al usar `MenuItemsSource`, establece `ItemTemplate` como un elemento NavigationViewItem y enlaza su propiedad `MenuItemsSource` al siguiente nivel de la jerarquía.

Aunque un elemento NavigationViewItem puede contener cualquier número de niveles anidados, se recomienda mantener una jerarquía de navegación de la aplicación superficial. Creemos que dos niveles son ideales para facilitar el uso y la comprensión.

NavigationView muestra la jerarquía en los modos de visualización del panel Top, Left y LeftCompact. Este es el aspecto que tiene un subárbol expandido en cada uno de los modos de visualización del panel:

![NavigationView con jerarquía](images/navigation-view-hierarchy-labeled.png)

### <a name="adding-a-hierarchy-of-items-in-markup"></a>Adición de una jerarquía de elementos en el marcado
Declara la jerarquía de navegación de la aplicación en el marcado.

```Xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:NavigationView>
    <muxc:NavigationView.MenuItems>
        <muxc:NavigationViewItem Content="Home" Icon="Home" ToolTipService.ToolTip="Home"/>
        <muxc:NavigationViewItem Content="Collections" Icon="Keyboard" ToolTipService.ToolTip="Collections">
            <muxc:NavigationViewItem.MenuItems>
                <muxc:NavigationViewItem Content="Notes" Icon="Page" ToolTipService.ToolTip="Notes"/>
                <muxc:NavigationViewItem Content="Mail" Icon="Mail" ToolTipService.ToolTip="Mail"/>
            </muxc:NavigationViewItem.MenuItems>
        </muxc:NavigationViewItem>
    </muxc:NavigationView.MenuItems>
</muxc:NavigationView>
```

### <a name="adding-a-hierarchy-of-items-using-data-binding"></a>Adición de una jerarquía de elementos mediante el enlace de datos

Para agregar una jerarquía de elementos de menú a NavigationView, realiza las siguientes acciones: 
* Enlazar la propiedad MenuItemsSource a los datos jerárquicos.
* Definir la plantilla de elemento como un elemento NavigationViewMenuItem, con el contenido establecido en la etiqueta del elemento de menú y la propiedad MenuItemsSource enlazada al siguiente nivel de la jerarquía.

En este ejemplo también se muestran los eventos [Expanding](/uwp/api/microsoft.ui.xaml.controls.navigationview.expanding?view=winui-2.4) y [Collapsed](/uwp/api/microsoft.ui.xaml.controls.navigationview.collapsed?view=winui-2.4). Estos eventos se generan para un elemento de menú con elementos secundarios.

```xaml
<Page ... xmlns:muxc="using:Microsoft.UI.Xaml.Controls" ... >
    <Page.Resources>
        <DataTemplate x:Key="NavigationViewMenuItem" x:DataType="local:Category">
            <muxc:NavigationViewItem Content="{x:Bind Name}" MenuItemsSource="{x:Bind Children}"/>
        </DataTemplate>
    </Page.Resources>

    <Grid>
        <muxc:NavigationView x:Name="navview" 
    MenuItemsSource="{x:Bind Categories, Mode=OneWay}" 
    MenuItemTemplate="{StaticResource NavigationViewMenuItem}" 
    ItemInvoked="{x:Bind OnItemInvoked}" 
    Expanding="OnItemExpanding" 
    Collapsed="OnItemCollapsed" 
    PaneDisplayMode="Left">
            <StackPanel Margin="10,10,0,0">
                <TextBlock Margin="0,10,0,0" x:Name="ExpandingItemLabel" Text="Last Expanding: N/A"/>
                <TextBlock x:Name="CollapsedItemLabel" Text="Last Collapsed: N/A"/>
            </StackPanel>
        </muxc:NavigationView>
    </Grid>
</Page>
```

```csharp
public class Category
{
    public String Name { get; set; }
    public String CategoryIcon { get; set; }
    public ObservableCollection<Category> Children { get; set; }
}

public sealed partial class HierarchicalNavigationViewDataBinding : Page
{
    public HierarchicalNavigationViewDataBinding()
    {
        this.InitializeComponent();
    }  

    public ObservableCollection<Category> Categories = new ObservableCollection<Category>()
    {
        new Category(){
            Name = "Menu item 1",
            CategoryIcon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu item 2",
                    CategoryIcon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() {
                            Name  = "Menu item 3",
                            CategoryIcon = "Icon",
                            Children = new ObservableCollection<Category>() {
                                new Category() { Name  = "Menu item 4", CategoryIcon = "Icon" },
                                new Category() { Name  = "Menu item 5", CategoryIcon = "Icon" }
                            }
                        }
                    }
                }
            }
        },
        new Category(){
            Name = "Menu item 6",
            CategoryIcon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu item 7",
                    CategoryIcon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() { Name  = "Menu item 8", CategoryIcon = "Icon" },
                        new Category() { Name  = "Menu item 9", CategoryIcon = "Icon" }
                    }
                }
            }
        },
        new Category(){ Name = "Menu item 10", CategoryIcon = "Icon" }
    };

    private void OnItemInvoked(object sender, NavigationViewItemInvokedEventArgs e)
    {
        var clickedItem = e.InvokedItem;
        var clickedItemContainer = e.InvokedItemContainer;
    }
    private void OnItemExpanding(object sender, NavigationViewItemExpandingEventArgs e)
    {
        var nvib = e.ExpandingItemContainer;
        var name = "Last expanding: " + nvib.Content.ToString();
        ExpandingItemLabel.Text = name;
    }
    private void OnItemCollapsed(object sender, NavigationViewItemCollapsedEventArgs e)
    {
        var nvib = e.CollapsedItemContainer;
        var name = "Last collapsed: " + nvib.Content;
        CollapsedItemLabel.Text = name;
    }
}
```

```cppwinrt
// Category.idl
namespace HierarchicalNavigationViewDataBinding
{
    runtimeclass Category
    {
        String Name;
        String CategoryIcon;
        Windows.Foundation.Collections.IObservableVector<Category> Children;
    }
}

// Category.h
#pragma once
#include "Category.g.h"

namespace winrt::HierarchicalNavigationViewDataBinding::implementation
{
    struct Category : CategoryT<Category>
    {
        Category();
        Category(winrt::hstring name,
            winrt::hstring categoryIcon,
            Windows::Foundation::Collections::
                IObservableVector<HierarchicalNavigationViewDataBinding::Category> children);

        winrt::hstring Name();
        void Name(winrt::hstring const& value);
        winrt::hstring CategoryIcon();
        void CategoryIcon(winrt::hstring const& value);
        Windows::Foundation::Collections::
            IObservableVector<HierarchicalNavigationViewDataBinding::Category> Children();
        void Children(Windows::Foundation::Collections:
            IObservableVector<HierarchicalNavigationViewDataBinding::Category> const& value);

    private:
        winrt::hstring m_name;
        winrt::hstring m_categoryIcon;
        Windows::Foundation::Collections::
            IObservableVector<HierarchicalNavigationViewDataBinding::Category> m_children;
    };
}

// Category.cpp
#include "pch.h"
#include "Category.h"
#include "Category.g.cpp"

namespace winrt::HierarchicalNavigationViewDataBinding::implementation
{
    Category::Category()
    {
        m_children = winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    }

    Category::Category(
        winrt::hstring name,
        winrt::hstring categoryIcon,
        Windows::Foundation::Collections::
            IObservableVector<HierarchicalNavigationViewDataBinding::Category> children)
    {
        m_name = name;
        m_categoryIcon = categoryIcon;
        m_children = children;
    }

    hstring Category::Name()
    {
        return m_name;
    }

    void Category::Name(hstring const& value)
    {
        m_name = value;
    }

    hstring Category::CategoryIcon()
    {
        return m_categoryIcon;
    }

    void Category::CategoryIcon(hstring const& value)
    {
        m_categoryIcon = value;
    }

    Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category>
        Category::Children()
    {
        return m_children;
    }

    void Category::Children(
        Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category>
            const& value)
    {
        m_children = value;
    }
}

// MainPage.idl
import "Category.idl";

namespace HierarchicalNavigationViewDataBinding
{
    [default_interface]
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        Windows.Foundation.Collections.IObservableVector<Category> Categories{ get; };
    }
}

// MainPage.h
#pragma once

#include "MainPage.g.h"

namespace muxc
{
    using namespace winrt::Microsoft::UI::Xaml::Controls;
};

namespace winrt::HierarchicalNavigationViewDataBinding::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category>
            Categories();

        void OnItemInvoked(muxc::NavigationView const& sender, muxc::NavigationViewItemInvokedEventArgs const& args);
        void OnItemExpanding(
            muxc::NavigationView const& sender,
            muxc::NavigationViewItemExpandingEventArgs const& args);
        void OnItemCollapsed(
            muxc::NavigationView const& sender,
            muxc::NavigationViewItemCollapsedEventArgs const& args);

    private:
        Windows::Foundation::Collections::
            IObservableVector<HierarchicalNavigationViewDataBinding::Category> m_categories;
    };
}

namespace winrt::HierarchicalNavigationViewDataBinding::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}

// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

#include "Category.h"

namespace winrt::HierarchicalNavigationViewDataBinding::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();

        m_categories = 
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();

        auto menuItem10 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 10", L"Icon", nullptr);

        auto menuItem9 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 9", L"Icon", nullptr);
        auto menuItem8 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 8", L"Icon", nullptr);
        auto menuItem7Children = 
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
        menuItem7Children.Append(*menuItem9);
        menuItem7Children.Append(*menuItem8);

        auto menuItem7 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 7", L"Icon", menuItem7Children);
        auto menuItem6Children = 
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
        menuItem6Children.Append(*menuItem7);

        auto menuItem6 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 6", L"Icon", menuItem6Children);

        auto menuItem5 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 5", L"Icon", nullptr);
        auto menuItem4 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 4", L"Icon", nullptr);
        auto menuItem3Children =
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
        menuItem3Children.Append(*menuItem5);
        menuItem3Children.Append(*menuItem4);

        auto menuItem3 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 3", L"Icon", menuItem3Children);
        auto menuItem2Children = 
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
        menuItem2Children.Append(*menuItem3);

        auto menuItem2 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 2", L"Icon", menuItem2Children);
        auto menuItem1Children = 
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
        menuItem1Children.Append(*menuItem2);

        auto menuItem1 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 1", L"Icon", menuItem1Children);

        m_categories.Append(*menuItem1);
        m_categories.Append(*menuItem6);
        m_categories.Append(*menuItem10);
    }

    Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category>
        MainPage::Categories()
    {
        return m_categories;
    }

    void MainPage::OnItemInvoked(
        muxc::NavigationView const& /* sender */,
        muxc::NavigationViewItemInvokedEventArgs const& args)
    {
        auto clickedItem = args.InvokedItem();
        auto clickedItemContainer = args.InvokedItemContainer();
    }

    void MainPage::OnItemExpanding(
        muxc::NavigationView const& /* sender */,
        muxc::NavigationViewItemExpandingEventArgs const& args)
    {
        auto nvib = args.ExpandingItemContainer();
        auto name = L"Last expanding: " + winrt::unbox_value<winrt::hstring>(nvib.Content());
        ExpandingItemLabel().Text(name);
    }

    void MainPage::OnItemCollapsed(
        muxc::NavigationView const& /* sender */,
        muxc::NavigationViewItemCollapsedEventArgs const& args)
    {
        auto nvib = args.CollapsedItemContainer();
        auto name = L"Last collapsed: " + winrt::unbox_value<winrt::hstring>(nvib.Content());
        CollapsedItemLabel().Text(name);
    }
}
```

### <a name="selection"></a>Selección

De forma predeterminada, cualquier elemento puede contener elementos secundarios, invocarse o seleccionarse.

Al proporcionar a los usuarios un árbol jerárquico de opciones de navegación, puedes optar por hacer que los elementos primarios no se puedan seleccionar, por ejemplo, si la aplicación no tiene una página de destino asociada a ese elemento primario. Si los elementos primarios _son_ seleccionables, se recomienda usar los modos de visualización del panel Left-Expanded o Top. El modo LeftCompact hará que el usuario navegue hasta el elemento primario para abrir el subárbol secundario cada vez que se invoque.

Los elementos seleccionados dibujarán sus indicadores de selección a lo largo de su borde izquierdo en el modo Left o en el borde inferior en el modo Top. A continuación se muestran los elementos NavigationView en los modos Left i Top donde está seleccionado un elemento primario.

![NavigationView en modo Left con un elemento primario seleccionado](images/navigation-view-selection.png)

![NavigationView en modo Top con un elemento primario seleccionado](images/navigation-view-selection-top.png)

Es posible que el elemento seleccionado no siempre permanezca visible. Si se selecciona un elemento secundario en un subárbol contraído o no expandido, su primer antecesor visible se mostrará como seleccionado. El indicador de selección se devolverá al elemento seleccionado si el subárbol está expandido.

Por ejemplo, en la imagen anterior, el usuario puede seleccionar el elemento Calendario y, a continuación, contraer su subárbol. En este caso, el indicador de selección se mostraría debajo del elemento Cuenta, ya que este es el primer antecesor visible de Calendario. El indicador de selección se devolverá al elemento Calendario cuando el usuario vuelva a expandir el subárbol. 

El elemento NavigationView completo no mostrará más de un indicador de selección.

En los modos Top y Left, al hacer clic en las flechas de los elementos NavigationViewItem, se expandirá o contraerá el subárbol. Al hacer clic o pulsar en _otro lugar_ en el elemento NavigationViewItem se desencadenará el evento `ItemInvoked` y también se contraerá o expandirá el subárbol.

Para evitar que un elemento muestre el indicador de selección cuando se invoca, establece su propiedad [SelectsOnInvoked](/uwp/api/microsoft.ui.xaml.controls.navigationviewitem.selectsoninvoked?view=winui-2.3) en False, como se muestra a continuación:

```xaml
<Page ... xmlns:muxc="using:Microsoft.UI.Xaml.Controls" ... >
    <Page.Resources>
        <DataTemplate x:Key="NavigationViewMenuItem" x:DataType="local:Category">
            <muxc:NavigationViewItem Content="{x:Bind Name}"
            MenuItemsSource="{x:Bind Children}"
            SelectsOnInvoked="{x:Bind IsLeaf}"/>
        </DataTemplate>
    </Page.Resources>

    <Grid>
        <muxc:NavigationView x:Name="navview" 
    MenuItemsSource="{x:Bind Categories, Mode=OneWay}" 
    MenuItemTemplate="{StaticResource NavigationViewMenuItem}">
        </muxc:NavigationView>
    </Grid>
</Page>
```

```csharp
public class Category
{
    public String Name { get; set; }
    public String CategoryIcon { get; set; }
    public ObservableCollection<Category> Children { get; set; }
    public bool IsLeaf { get; set; }
}

public sealed partial class HierarchicalNavigationViewDataBinding : Page
{
    public HierarchicalNavigationViewDataBinding()
    {
        this.InitializeComponent();
    }  

    public ObservableCollection<Category> Categories = new ObservableCollection<Category>()
    {
        new Category(){
            Name = "Menu item 1",
            CategoryIcon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu item 2",
                    CategoryIcon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() {
                            Name  = "Menu item 3",
                            CategoryIcon = "Icon",
                            Children = new ObservableCollection<Category>() {
                                new Category() { Name  = "Menu item 4", CategoryIcon = "Icon", IsLeaf = true },
                                new Category() { Name  = "Menu item 5", CategoryIcon = "Icon", IsLeaf = true }
                            }
                        }
                    }
                }
            }
        },
        new Category(){
            Name = "Menu item 6",
            CategoryIcon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu item 7",
                    CategoryIcon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() { Name  = "Menu item 8", CategoryIcon = "Icon", IsLeaf = true },
                        new Category() { Name  = "Menu item 9", CategoryIcon = "Icon", IsLeaf = true }
                    }
                }
            }
        },
        new Category(){ Name = "Menu item 10", CategoryIcon = "Icon", IsLeaf = true }
    };
}
```

```cppwinrt
// Category.idl
namespace HierarchicalNavigationViewDataBinding
{
    runtimeclass Category
    {
        ...
        Boolean IsLeaf;
    }
}

// Category.h
...
struct Category : CategoryT<Category>
{
    ...
    Category(winrt::hstring name,
        winrt::hstring categoryIcon,
        Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category> children,
        bool isleaf = false);
    ...
    bool IsLeaf();
    void IsLeaf(bool value);

private:
    ...
    bool m_isleaf;
};

// Category.cpp
...
Category::Category(winrt::hstring name,
    winrt::hstring categoryIcon,
    Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category> children,
    bool isleaf) : m_name(name), m_categoryIcon(categoryIcon), m_children(children), m_isleaf(isleaf) {}
...
bool Category::IsLeaf()
{
    return m_isleaf;
}

void Category::IsLeaf(bool value)
{
    m_isleaf = value;
}

// MainPage.h and MainPage.cpp
// Delete OnItemInvoked, OnItemExpanding, and OnItemCollapsed.

// MainPage.cpp
...
MainPage::MainPage()
{
    InitializeComponent();

    m_categories = winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();

    auto menuItem10 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 10", L"Icon", nullptr, true);

    auto menuItem9 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 9", L"Icon", nullptr, true);
    auto menuItem8 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 8", L"Icon", nullptr, true);
    auto menuItem7Children = 
        winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    menuItem7Children.Append(*menuItem9);
    menuItem7Children.Append(*menuItem8);

    auto menuItem7 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 7", L"Icon", menuItem7Children);
    auto menuItem6Children = 
        winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    menuItem6Children.Append(*menuItem7);

    auto menuItem6 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 6", L"Icon", menuItem6Children);

    auto menuItem5 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 5", L"Icon", nullptr, true);
    auto menuItem4 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 4", L"Icon", nullptr, true);
    auto menuItem3Children = 
        winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    menuItem3Children.Append(*menuItem5);
    menuItem3Children.Append(*menuItem4);

    auto menuItem3 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 3", L"Icon", menuItem3Children);
    auto menuItem2Children = 
        winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    menuItem2Children.Append(*menuItem3);

    auto menuItem2 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 2", L"Icon", menuItem2Children);
    auto menuItem1Children = 
        winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    menuItem1Children.Append(*menuItem2);

    auto menuItem1 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 1", L"Icon", menuItem1Children);

    m_categories.Append(*menuItem1);
    m_categories.Append(*menuItem6);
    m_categories.Append(*menuItem10);
}
...
```

### <a name="keyboarding-within-hierarchical-navigationview"></a>Uso del teclado con el elemento NavigationView jerárquico

Los usuarios pueden cambiar el foco en la vista de navegación mediante el [teclado](../input/keyboard-interactions.md). Las teclas de dirección exponen la "navegación interna" dentro del panel y siguen las interacciones que se proporcionan en la [vista de árbol](./tree-view.md). Las acciones clave cambian al desplazarse por el elemento NavigationView o su menú flotante, que se muestra en los modos Top y Left-Compact de HierarchicalNavigationView. A continuación se muestran las acciones específicas que puede realizar cada clave en un elemento NavigationView jerárquico:

| Clave      |      En el modo Left      |  En el modo Top | En el modo de control flotante  |
|----------|------------------------|--------------|------------|
| Subir |Mueve el foco al elemento directamente encima del elemento que está en el foco actualmente. | No hace nada. |Mueve el foco al elemento directamente encima del elemento que está en el foco actualmente.|
| Bajar|Mueve el foco directamente debajo del elemento que está en el foco actualmente.* | No hace nada. | Mueve el foco directamente debajo del elemento que está en el foco actualmente.* |
| Derecha |No hace nada.  |Mueve el foco al elemento directamente a la derecha del elemento que está en el foco actualmente. |No hace nada.|
| Izquierda |No hace nada. | Mueve el foco al elemento directamente a la izquierda del elemento que está en el foco actualmente.  |No hace nada. |
| Espacio/Entrar |Si el elemento tiene elementos secundarios, expande o contrae el elemento y no cambia el foco.   | Si el elemento tiene elementos secundarios, expande los elementos secundarios en un control flotante y coloca el foco en el primer elemento del control flotante. | Invoca o selecciona el elemento y cierra el control flotante. |
| Esc | No hace nada. | No hace nada. | Cierra el control flotante.|

La barra espaciadora o la tecla Entrar siempre invocan o seleccionan un elemento.

*Ten en cuenta que no es necesario que los elementos sean adyacentes visualmente. El foco se desplazará del último elemento de la lista del panel al elemento de configuración. 

## <a name="navigation-view-customization"></a>Personalización de la vista de navegación

### <a name="pane-backgrounds"></a>Fondos del panel

De forma predeterminada, el panel NavigationView usa un fondo diferente según el modo de pantalla:

- el panel es de color gris sólido cuando se expande a la izquierda, en paralelo con el contenido (en modo izquierdo).
- el panel usa acrílico en la aplicación cuando se abre como una superposición sobre el contenido (en modo superior, mínimo o compacto).

Para modificar el fondo del panel, puedes reemplazar los recursos de tema XAML usados para representar el fondo en cada modo. (Esta técnica se usa en lugar de una sola propiedad PaneBackground con el fin de admitir distintos fondos para distintos modos de pantalla).

En esta tabla se muestra qué recurso de tema se usa en cada modo de pantalla.

| Modo de pantalla | Recursos de tema |
| ------------ | -------------- |
| Izquierda | NavigationViewExpandedPaneBackground |
| LeftCompact<br/>LeftMinimal | NavigationViewDefaultPaneBackground |
| Superior | NavigationViewTopPaneBackground |

En este ejemplo se muestra cómo invalidar los recursos de tema en App.xaml. Cuando invalides los recursos de tema, debes siempre proporcionar como mínimo los diccionarios de recursos "Default" y "HighContrast" y los diccionarios de recursos "Light" o "Dark" según sea necesario. Para más información, consulta [ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries).

> [!IMPORTANT]
> Este código muestra cómo usar la versión de AcrylicBrush de la [biblioteca de interfaz de usuario de Windows](/uwp/toolkits/winui/). Si usas en cambio la versión de AcrylicBrush de la plataforma, la versión mínima del proyecto de aplicación debe ser SDK 16299 o superior. Para usar la versión de la plataforma, quita todas las referencias a `muxm:`.

```xaml
<Application ... xmlns:muxm="using:Microsoft.UI.Xaml.Media" ...>
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

### <a name="top-whitespace"></a>Espacio en blanco superior

> La propiedad `IsTitleBarAutoPaddingEnabled` requiere la [biblioteca de interfaz de usuario de Windows](/uwp/toolkits/winui/) versión 2.2 o posterior.

Algunas aplicaciones permiten [personalizar la barra de título de sus ventanas](../shell/title-bar.md). Esto permite extender el contenido de la aplicación al área de la barra de título. Cuando NavigationView es el elemento raíz de las aplicaciones que se pueden extender a la barra de título  **mediante [ExtendViewIntoTitleBar](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.extendviewintotitlebar) API**, el control ajusta automáticamente la posición de los elementos interactivos para evitar la superposición con [la región arrastrable](../shell/title-bar.md#draggable-regions).

![Una aplicación que se extiende a la barra de título](images/navigation-view-with-titlebar-padding.png)

Si la aplicación especifica la región arrastrable mediante una llamada al método [Window.SetTitleBar](/uwp/api/windows.ui.xaml.window.settitlebar) y prefiere que los botones Atrás y Menú se acerquen a la parte superior de la ventana de la aplicación, establezca [IsTitleBarAutoPaddingEnabled](/uwp/api/microsoft.ui.xaml.controls.navigationview.istitlebarautopaddingenabled) en **false**.

![Aplicación que se extiende a la barra de título sin relleno adicional](images/navigation-view-no-titlebar-padding.png)

```Xaml
<muxc:NavigationView x:Name="NavView" IsTitleBarAutoPaddingEnabled="False">
```

#### <a name="remarks"></a>Observaciones
Para ajustar aún más la posición del área de encabezado de NavigationView, invalida, por ejemplo, el recurso de tema XAML de *NavigationViewHeaderMargin* en los recursos de la página.

```Xaml
<Page.Resources>
    <Thickness x:Key="NavigationViewHeaderMargin">12,0</Thickness>
</Page.Resources>
```

Este recurso de tema modifica el margen alrededor de [NavigationView.Header](/uwp/api/windows.ui.xaml.controls.navigationview.header).

## <a name="related-topics"></a>Temas relacionados

- [Clase NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview)
- [Maestro/detalles](master-details.md)
- [Conceptos básicos de navegación](../basics/navigation-basics.md)
- [Introducción a Fluent Design](/windows/apps/fluent-design-system)
