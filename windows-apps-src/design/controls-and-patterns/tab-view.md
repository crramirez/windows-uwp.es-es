---
Description: TabView es una manera flexible de organizar varios documentos en pestañas dinámicas.
title: Vista de pestañas
template: detail.hbs
ms.date: 09/12/2019
ms.topic: article
keywords: windows 10, uwp
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f5ec09f8fdfa0a8b4b8c7161cc6d91a3013230d7
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970690"
---
# <a name="tabview"></a>TabView

El control TabView es una manera de mostrar un conjunto de pestañas y su contenido correspondiente. Los controles TabView son útiles para mostrar varias páginas (o documentos) de contenido, a la vez que proporcionan a los usuarios la capacidad de reorganizar, abrir o cerrar nuevas pestañas.

![Ejemplo de TabView](images/tabview/tab-introduction.png)

**Obtención de la biblioteca de la interfaz de usuario de Windows**

|  |  |
| - | - |
| ![Logotipo de WinUI](images/winui-logo-64x64.png) | El control **TabView** requiere la biblioteca de interfaz de usuario de Windows, un paquete NuGet que contiene nuevos controles y características de interfaz de usuario destinados a aplicaciones de Windows. Para obtener más información e instrucciones sobre la instalación, consulta el artículo [Windows UI Library](https://docs.microsoft.com/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows). |

> **API de la biblioteca de interfaz de usuario de Windows**: [Clase TabView](/uwp/api/microsoft.ui.xaml.controls.tabview), [clase TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem)

> [!TIP]
> En este documento, se usa el alias **muxc** en XAML para representar las API de la biblioteca de interfaz de usuario de Windows que hemos incluido en nuestro proyecto. Hemos agregado lo siguiente a nuestro elemento [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page): `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>En el código subyacente, se usa el alias **muxc** en C# para representar las API de la biblioteca de interfaz de usuario de Windows que hemos incluido en nuestro proyecto. Hemos agregado esta instrucción **using** en la parte superior del archivo: `using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

En general, las UI con pestañas vienen en uno de dos estilos distintos que difieren en función y aspecto: Las **pestañas estáticas** son el tipo de pestaña que se encuentra a menudo en las ventanas de configuración. Contienen un número establecido de páginas en un orden fijo que normalmente incluyen contenido predefinido.
Las **pestañas de documentos** son el tipo de pestaña que se encuentra en un explorador, como Microsoft Edge. Los usuarios pueden crear, quitar y reorganizar las pestañas, desplazar las pestañas entre ventanas y cambiar el contenido de las pestañas.

[TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) ofrece pestañas de documentos para aplicaciones para UWP. Usa un control TabView cuando:

- Los usuarios puedan abrir, cerrar o reorganizar las pestañas dinámicamente.
- Los usuarios puedan abrir documentos o páginas web directamente en pestañas.
- Los usuarios puedan arrastrar y colocar pestañas entre ventanas.

Si un control TabView no es adecuado para tu aplicación, considera la posibilidad de usar controles como [Pivot](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/pivot) o [NavigationView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview).

## <a name="anatomy"></a>Anatomía

En la imagen siguiente se muestran las partes del control [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview). TabStrip tiene un encabezado y un pie de página, pero a diferencia de un documento, el encabezado y el pie de página de TabStrip se encuentran en el extremo izquierdo y el extremo derecho de la banda, respectivamente.

![Anatomía del control TabView](images/tabview/tab-view-anatomy.png)

En la imagen siguiente se muestran las partes del control [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem). Ten en cuenta que aunque el contenido se muestra dentro del control TabView, el contenido en realidad forma parte de TabViewItem.

![Anatomía del control TabViewItem](images/tabview/tab-control-anatomy.png)

### <a name="create-a-tab-view"></a>Crear una vista de pestañas

En este ejemplo se crea un control [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) sencillo junto con controladores de eventos para admitir la apertura y el cierre de pestañas.

```xaml
 <muxc:TabView AddTabButtonClick="TabView_AddTabButtonClick"
               TabCloseRequested="TabView_TabCloseRequested"/>
```

```csharp
// Add a new Tab to the TabView
private void TabView_AddTabButtonClick(muxc.TabView sender, object args)
{
    var newTab = new muxc.TabViewItem();
    newTab.IconSource = new muxc.SymbolIconSource() { Symbol = Symbol.Document };
    newTab.Header = "New Document";

    // The Content of a TabViewItem is often a frame which hosts a page.
    Frame frame = new Frame();
    newTab.Content = frame;
    frame.Navigate(typeof(Page1));

    sender.TabItems.Add(newTab);
}

// Remove the requested tab from the TabView
private void TabView_TabCloseRequested(muxc.TabView sender, muxc.TabViewTabCloseRequestedEventArgs args)
{
    sender.TabItems.Remove(args.Tab);
}
```

## <a name="behavior"></a>Comportamiento

Hay varias maneras de aprovechar o ampliar la funcionalidad de un control [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview).

### <a name="bind-tabitemssource-to-a-tabviewitemcollection"></a>Enlazar TabItemsSource a TabViewItemCollection

```xaml
<muxc:TabView TabItemsSource="{x:Bind TabViewItemCollection}" />
```

### <a name="display-tabview-tabs-in-a-windows-titlebar"></a>Mostrar pestañas TabView en la barra de título de una ventana

En lugar de hacer que las pestañas ocupen su propia fila debajo de la barra de título de una ventana, puedes combinar las dos en la misma área. De este modo, se ahorra espacio vertical para el contenido y la aplicación adquiere un aspecto más moderno.

Dado que un usuario puede arrastrar una ventana por la barra de título para cambiar la posición de la ventana, es importante que la barra de título no se rellene completamente con pestañas. Por lo tanto, al mostrar las pestañas en una barra de título, tienes que especificar una parte de la barra de títuloque se va a reservar como área arrastrable. Si no especificas una región arrastrable, se podrá arrastrar toda la barra de título, lo que impedirá que las pestañas reciban eventos de entrada. Si el control TabView se muestra en la barra de título de una ventana, siempre tienes que incluir un elemento [TabStripFooter](/uwp/api/microsoft.ui.xaml.controls.tabview.tabstripfooter) en [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) y marcarlo como región arrastrable.

Para obtener más información, consulta [Personalización de la barra de título](https://docs.microsoft.com/windows/uwp/design/shell/title-bar).

![Pestañas en la barra de título](images/tabview/tab-extend-to-title.png)

```xaml
<muxc:TabView HorizontalAlignment="Stretch" VerticalAlignment="Stretch">
    <muxc:TabView.TabItems>
        <muxc:TabViewItem Header="Home" IsClosable="False">
            <muxc:TabViewItem.IconSource>
                <muxc:SymbolIconSource Symbol="Home" />
            </muxc:TabViewItem.IconSource>
        </muxc:TabViewItem>
        <muxc:TabViewItem Header="Document 1">
            <muxc:TabViewItem.IconSource>
                <muxc:SymbolIconSource Symbol="Document" />
            </muxc:TabViewItem.IconSource>
        </muxc:TabViewItem>
        <muxc:TabViewItem Header="Document 2">
            <muxc:TabViewItem.IconSource>
                <muxc:SymbolIconSource Symbol="Document" />
            </muxc:TabViewItem.IconSource>
        </muxc:TabViewItem>
        <muxc:TabViewItem Header="Document 3">
            <muxc:TabViewItem.IconSource>
                <muxc:SymbolIconSource Symbol="Document" />
            </muxc:TabViewItem.IconSource>
        </muxc:TabViewItem>
    </muxc:TabView.TabItems>

    <muxc:TabView.TabStripHeader>
        <Grid x:Name="ShellTitlebarInset" Background="Transparent" />
    </muxc:TabView.TabStripHeader>
    <muxc:TabView.TabStripFooter>
        <Grid x:Name="CustomDragRegion" Background="Transparent" />
    </muxc:TabView.TabStripFooter>
</muxc:TabView>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;
    coreTitleBar.LayoutMetricsChanged += CoreTitleBar_LayoutMetricsChanged;

    Window.Current.SetTitleBar(CustomDragRegion);
}

private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (FlowDirection == FlowDirection.LeftToRight)
    {
        CustomDragRegion.MinWidth = sender.SystemOverlayRightInset;
        ShellTitlebarInset.MinWidth = sender.SystemOverlayLeftInset;
    }
    else
    {
        CustomDragRegion.MinWidth = sender.SystemOverlayLeftInset;
        ShellTitlebarInset.MinWidth = sender.SystemOverlayRightInset;
    }

    CustomDragRegion.Height = ShellTitlebarInset.Height = sender.Height;
}
```

>[!NOTE]
> Para asegurarte de que el contenido de shell no obstruya las pestañas de la barra de título, debes tener en cuenta las superposiciones de la izquierda y la derecha. En los diseños de izquierda a derecha (LTR), el bajorrelieve derecho incluye los botones de título y la región de arrastre. Lo inverso es verdadero en el diseño de derecha a izquierda (RTL). Los valores SystemOverlayLeftInset y SystemOverlayRightInset se definen según la izquierda y derecha físicas, así que también tienen que invertirse en RTL.

### <a name="control-overflow-behavior"></a>Controlar el comportamiento de desbordamiento

A medida que la barra de pestañas se llena de pestañas, puedes controlar cómo se muestran si estableces el valor [TabView.TabWidthMode](/uwp/api/microsoft.ui.xaml.controls.tabview.tabwidthmode).

| Valor TabWidthMode | Comportamiento                                                                                                                                                    |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Equal              | A medida que se agreguen nuevas pestañas, todas las pestañas se reducirán horizontalmente hasta que alcancen un ancho mínimo muy pequeño.                                                       |
| SizeToContent      | Las pestañas siempre tendrán su "tamaño natural", el tamaño mínimo necesario para mostrar su icono y encabezado. No se expandirán ni se reducirán a medida que se agreguen o cierren. |

Sea cual sea el valor que elijas, en algún momento es posible que haya demasiadas pestañas para mostrar en la franja de pestañas. En este caso, aparecerán botones de desplazamiento que permitirán al usuario desplazar TabStrip a la izquierda y a la derecha.

### <a name="guidance-for-tab-selection"></a>Guía para la selección de pestañas

La mayoría de los usuarios tiene experiencia en el uso de pestañas de documentos simplemente por el hecho de usar un explorador web. Cuando usen pestañas de documentos en tu aplicación, su experiencia condiciona sus expectativas acerca de cómo deben comportarse las pestañas.

Independientemente de cómo interactúe el usuario con un conjunto de pestañas de documento, siempre debe haber una pestaña activa. Si el usuario cierra la pestaña seleccionada o separa la pestaña seleccionada en otra ventana, otra pestaña debe convertirse en la pestaña activa. Para ello, [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) selecciona automáticamente la pestaña siguiente. Si tienes un buen motivo para que la aplicación permita un control TabView con una pestaña no seleccionada, el área de contenido de TabView sencillamente estará en blanco.

## <a name="keyboard-navigation"></a>Navegación con el teclado

De manera predeterminada, [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) admite muchos escenarios comunes de navegación por teclado. En esta sección se explica la funcionalidad integrada y se hacen recomendaciones sobre funcionalidades adicionales que pueden resultar útiles para algunas aplicaciones.

### <a name="tab-and-cursor-key-behavior"></a>Comportamiento de las teclas de tabulación y de cursor

Cuando el foco se desplaza al área de _TabStrip_, el elemento [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem) seleccionado recibe el foco. A continuación, el usuario puede usar las teclas de flecha izquierda y derecha para desplazar el foco (no la selección) a otras pestañas de TabStrip. El foco de las flechas queda atrapado en la franja de pestañas y el botón de agregar pestaña (+), si hay uno. Para sacar el foco del área de TabStrip, el usuario puede presionar la tecla TAB, que moverá el foco al siguiente elemento enfocable.

Desplazar el foco mediante TAB

![Desplazar el foco mediante TAB](images/tabview/tab-keyboard-behavior-1.png)

Las teclas de flecha no reinician el ciclo del foco.

![Las teclas de flecha no reinician el ciclo del foco.](images/tabview/tab-keyboard-behavior-3.png)

### <a name="selecting-a-tab"></a>Seleccionar una pestaña

Cuando el foco está en un elemento TabViewItem, al presionar la tecla de barra espaciadora o Entrar, se seleccionará dicho elemento TabViewItem.

Usa las teclas de dirección para desplazar el foco y, luego, presiona la barra espaciadora para seleccionar la pestaña.

![Barra espaciadora para seleccionar la pestaña](images/tabview/tab-keyboard-behavior-2.png)

### <a name="shortcuts-for-selecting-adjacent-tabs"></a>Accesos directos para seleccionar pestañas adyacentes

Con Control + TAB se selecciona el siguiente elemento [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem). Con Control + Mayús + TAB se selecciona el elemento TabViewItem anterior. A tales efectos, la lista de pestañas se recorre en bucle, por lo que si seleccionas la pestaña siguiente mientras está seleccionada la última pestaña, se seleccionará la primera pestaña.

### <a name="closing-a-tab"></a>Cerrar una pestaña

Si presionas Control + F4, se generará el evento [TabCloseRequested](/uwp/api/microsoft.ui.xaml.controls.tabview.tabcloserequested). Controla dicho evento y cierra la pestaña si es necesario.

### <a name="keyboard-guidance-for-app-developers"></a>Guía del teclado para desarrolladores de aplicaciones

Algunas aplicaciones pueden requerir un control de teclado más avanzado. Ten en cuenta la posibilidad de implementar los siguientes accesos directos si son adecuados para tu aplicación.

> [!WARNING]
> Si vas a agregar un control [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) a una aplicación existente, es posible que ya hayas creado métodos abreviados de teclado que se asignen a las combinaciones de teclas de los métodos abreviados de teclado de TabView recomendados. En este caso, tendrás que decidir si quieres mantener los accesos directos existentes u ofrecer al usuario una experiencia de pestañas intuitiva.

- La combinación de teclas Control + T debe abrir una nueva pestaña. Por lo general, esta pestaña se rellena con un documento predefinido o se crea vacía con una forma sencilla de elegir su contenido. Si el usuario debe elegir contenido para una nueva pestaña, ten en cuenta la posibilidad de poner el foco de entrada en el control de selección de contenido.
- La combinación Control + W debe cerrar la pestaña seleccionada. Recuerda que TabView seleccionará automáticamente la pestaña siguiente.
- La combinación de teclas Control + Mayús + T debe abrir pestañas cerradas recientemente (o más exactamente, abrir nuevas pestañas con el mismo contenido que las pestañas cerradas recientemente). Comienza con la pestaña cerrada más recientemente y retrocede en el tiempo para cada vez que se invoque al método abreviado. Ten en cuenta que esto exigirá mantener una lista de pestañas cerradas recientemente.
- La combinación de teclas Control + 1 debe seleccionar la primera pestaña de la lista de pestañas. Del mismo modo, con Control + 2 se debe seleccionar la segunda pestaña, con Control + 3, la tercera y así sucesivamente hasta Control + 8.
- La combinación de teclas Control + 9 debe seleccionar la última pestaña de la lista de pestañas, independientemente del número de pestañas que haya en la lista.
- Si las pestañas ofrecen más que simplemente el comando cerrar (como la duplicación o el anclaje de una pestaña), usa un menú contextual para mostrar todas las acciones disponibles que se pueden realizar en una pestaña.

### <a name="implement-browser-style-keyboarding-behavior"></a>Implementar un comportamiento de teclado al estilo del explorador

En este ejemplo, se implementan varias de las recomendaciones anteriores en un control [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview). En concreto, en este ejemplo se implementan Control + T, Control + W, Control + 1-8 y Control + 9.

```xaml
<muxc:TabView x:Name="TabRoot">
    <muxc:TabView.KeyboardAccelerators>
        <KeyboardAccelerator Key="T" Modifiers="Control" Invoked="NewTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="W" Modifiers="Control" Invoked="CloseSelectedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number1" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number2" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number3" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number4" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number5" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number6" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number7" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number8" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number9" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
    </muxc:TabView.KeyboardAccelerators>
    <!-- ... some tabs ... -->
</muxc:TabView>
```

```csharp
private void NewTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    // Create new tab.
    var newTab = new muxc.TabViewItem();
    newTab.IconSource = new muxc.SymbolIconSource() { Symbol = Symbol.Document };
    newTab.Header = "New Document";

    // The Content of a TabViewItem is often a frame which hosts a page.
    Frame frame = new Frame();
    newTab.Content = frame;
    frame.Navigate(typeof(Page1));

    TabRoot.TabItems.Add(newTab);
}

private void CloseSelectedTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    // Only remove the selected tab if it can be closed.
    if (((muxc.TabViewItem)TabRoot.SelectedItem).IsClosable)
    {
        TabRoot.TabItems.Remove(TabRoot.SelectedItem);
    }
}

private void NavigateToNumberedTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    int tabToSelect = 0;

    switch (sender.Key)
    {
        case Windows.System.VirtualKey.Number1:
            tabToSelect = 0;
            break;
        case Windows.System.VirtualKey.Number2:
            tabToSelect = 1;
            break;
        case Windows.System.VirtualKey.Number3:
            tabToSelect = 2;
            break;
        case Windows.System.VirtualKey.Number4:
            tabToSelect = 3;
            break;
        case Windows.System.VirtualKey.Number5:
            tabToSelect = 4;
            break;
        case Windows.System.VirtualKey.Number6:
            tabToSelect = 5;
            break;
        case Windows.System.VirtualKey.Number7:
            tabToSelect = 6;
            break;
        case Windows.System.VirtualKey.Number8:
            tabToSelect = 7;
            break;
        case Windows.System.VirtualKey.Number9:
            // Select the last tab
            tabToSelect = TabRoot.TabItems.Count - 1;
            break;
    }

    // Only select the tab if it is in the list
    if (tabToSelect < TabRoot.TabItems.Count)
    {
        TabRoot.SelectedIndex = tabToSelect;
    }
}
```

## <a name="related-articles"></a>Artículos relacionados

- [MasterDetails](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/master-details)
- [NavigationView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview)
- [Pivot](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/pivot)
