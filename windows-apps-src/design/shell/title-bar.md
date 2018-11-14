---
author: jwmsft
description: Personalizar la barra de título de una aplicación de escritorio para que coincida con la personalidad de la aplicación.
title: Personalización de la barra de título
template: detail.hbs
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, barra de título
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 2ebe590f98afef031ab183589fc7dcfc29cd9493
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "6252560"
---
# <a name="title-bar-customization"></a>Personalización de la barra de título



Cuando la aplicación se ejecuta en una ventana del escritorio, puedes personalizar las barras de título para que coincidan con la personalidad de tu aplicación. La personalización de la barra de título de la API te permite especificar colores para elementos de la barra de título, o ampliar el contenido de tu aplicación en el área de la barra de título y tomar el control completo de la misma.

> **API importantes**: [ propiedad ApplicationView.TitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview), [clase ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar), [clase CoreApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar)

## <a name="how-much-to-customize-the-title-bar"></a>Cuánto se puede personalizar la barra de título

Hay dos niveles de personalización que puedes aplicar a la barra de título.

Para la personalización de color sencilla, puedes establecer las propiedades [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) para especificar los colores que quiere1s usar para elementos de la barra de título. En este caso, el sistema conserva la responsabilidad para todos los demás aspectos de la barra de título, como el dibujo del nombre de la aplicación y la definición de áreas arrastrables.

La otra opción es ocultar la barra de título predeterminada y reemplazarla por su propio contenido XAML. Por ejemplo, puedes colocar texto, botones o menús personalizados en el área de la barra de título. También deberás usar esta opción para ampliar un fondo [acrílico](../style/acrylic.md) en el área de la barra de título.

Cuando eliges una personalización completa, eres responsable de colocar contenido en el área de la barra de título y puedes definir tu propia región arrastrable. Los botones Atrás, Cerrar, Minimizar y Maximizar del sistema todavía están disponibles y los controla el sistema, pero elementos como el título de la aplicación no lo están. Deberás crear esos elementos tú mismo según las necesidades de tu aplicación.

> [!NOTE]
> La personalización de color sencilla está disponible para aplicaciones para UWP con HTML, XAML y DirectX. La personalización completa solo está disponible para aplicaciones para UWP con XAML.

## <a name="simple-color-customization"></a>Personalización de color sencilla

Si solo quieres personalizar los colores de la barra de título y no hacer nada demasiado decorativo (por ejemplo, colocar pestañas en la barra de título), puedes establecer las propiedades de color en la [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) para la ventana de la aplicación.

En este ejemplo se muestra cómo obtener una instancia de ApplicationViewTitleBar y cómo establecer sus propiedades de color.

```csharp
// using Windows.UI.ViewManagement;

var titleBar = ApplicationView.GetForCurrentView().TitleBar;

// Set active window colors
titleBar.ForegroundColor = Windows.UI.Colors.White;
titleBar.BackgroundColor = Windows.UI.Colors.Green;
titleBar.ButtonForegroundColor = Windows.UI.Colors.White;
titleBar.ButtonBackgroundColor = Windows.UI.Colors.SeaGreen;
titleBar.ButtonHoverForegroundColor = Windows.UI.Colors.White;
titleBar.ButtonHoverBackgroundColor = Windows.UI.Colors.DarkSeaGreen;
titleBar.ButtonPressedForegroundColor = Windows.UI.Colors.Gray;
titleBar.ButtonPressedBackgroundColor = Windows.UI.Colors.LightGreen;

// Set inactive window colors
titleBar.InactiveForegroundColor = Windows.UI.Colors.Gray;
titleBar.InactiveBackgroundColor = Windows.UI.Colors.SeaGreen;
titleBar.ButtonInactiveForegroundColor = Windows.UI.Colors.Gray;
titleBar.ButtonInactiveBackgroundColor = Windows.UI.Colors.SeaGreen;
```

> [!NOTE]
> Este código se puede colocar en el método [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched) de tu aplicación (_App.xaml.cs_), después de la llamada a [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.Activate) o en la primera página de tu aplicación.

> [!TIP]
> El kit de herramientas de Comunidad Windows proporciona extensiones que te permiten establecer estas propiedades de color en XAML. Para obtener más información, consulta la [documentación del kit de herramientas de Comunidad Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/extensions/viewextensions).

Hay algunas cosas que debes tener en cuenta al establecer los colores de la barra de título:

- El color de fondo del botón no se aplica a los de estados de presionado y mantener el mouse del botón Cerrar. El botón Cerrar siempre usa el color definido por el sistema para esos estados.
- Las propiedades de color del botón se aplican al botón Atrás del sistema cuando se usa. ([Consulta Historial de navegación y navegación hacia atrás](../basics/navigation-history-and-backwards-navigation.md).)
- El establecimiento de una propiedad de color en **null** lo restablece al color del sistema predeterminado.
- No se pueden establecer colores transparentes. Se ignora el canal alfa del color.

Windows ofrece a un usuario la opción de aplicar su [color de énfasis](https://docs.microsoft.com/windows/uwp/style/color#accent-color) seleccionado a la barra de título. Si estableces cualquier color de la barra de título, te recomendamos que establezcas todos los colores de manera explícita. Esto garantiza que no hay ninguna combinación de colores no intencionada que se produzca debido a la configuración de color definida por el usuario.

## <a name="full-customization"></a>Personalización completa

Cuando eliges la personalización de la barra de título completa, el área de cliente de tu aplicación se amplía para cubrir toda la ventana, incluida el área de la barra de título. Eres responsable del dibujo y del control de entrada para toda la ventana, excepto los botones de título, que se superponen encima del lienzo de la aplicación.

Para ocultar la barra de título predeterminada y ampliar el contenido en el área de la barra de título, establece la propiedad [CoreApplicationViewTitleBar.ExtendViewIntoTitleBar](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar) en **true**.

Este ejemplo muestra cómo obtener el valor de CoreApplicationViewTitleBar y establecer la propiedad ExtendViewIntoTitleBar en **true**. Esto se puede realizar en el método [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched) de tu aplicación (_App.xaml.cs_) o en la primera página de tu aplicación.

```csharp
// using Windows.ApplicationModel.Core;

// Hide default title bar.
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
coreTitleBar.ExtendViewIntoTitleBar = true;
```

> [!TIP]
> Esta configuración se mantiene cuando tu app se cierra y se reinicia. En Visual Studio, si estableces ExtendViewIntoTitleBar **true**y, a continuación, quieres volver a la configuración predeterminada, debe establecerla explícitamente en **false** y ejecutar la app para sobrescribir la configuración persistente.

### <a name="draggable-regions"></a>Regiones arrastrables

La región arrastable de la barra de título define el lugar donde el usuario puede hacer clic y arrastrar para desplazar la ventana (en lugar de simplemente arrastrar contenido dentro del lienzo de la aplicación). La región arrastrable se especifica mediante una llamada al método [Window.SetTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.settitlebar) y el paso de un UIElement que define la región arrastable. (El UIElement a menudo es un panel que contiene otros elementos.)

Esta es la manera de establecer una cuadrícula de contenido como la región de la barra de título arrastrable. Este código se coloca en el XAML y código subyacente para la primera página de tu aplicación. Consulta la sección [Ejemplo de personalización completa](./title-bar.md#full-customization-example) para obtener el código completo.

```xaml
<Grid x:Name="AppTitleBar" Background="Transparent">
    <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
    <!-- Using padding columns instead of Margin ensures that the background
         paints the area under the caption control buttons (for transparent buttons). -->
    <Grid.ColumnDefinitions>
        <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
        <ColumnDefinition/>
        <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
    </Grid.ColumnDefinitions>
    <Image Source="Assets/Square44x44Logo.png" 
           Grid.Column="1" HorizontalAlignment="Left" 
           Width="20" Height="20" Margin="12,0"/>
    <TextBlock Text="Custom Title Bar" 
               Grid.Column="1" 
               Style="{StaticResource CaptionTextBlockStyle}" 
               Margin="44,8,0,0"/>
</Grid>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;

    // Set XAML element as a draggable region.
    AppTitleBar.Height = coreTitleBar.Height;
    Window.Current.SetTitleBar(AppTitleBar);
}
```

El UIElement (`AppTitleBar`) forma parte del XAML de tu aplicación. Puedes declarar y establecer la barra de título en una página raíz que no cambia, o declarar y establecer una región de la barra de título en cada página a la que puede navegar tu aplicación. Si la estableces en cada página, asegúrate de que la región arrastrable se mantienen coherente cuando los usuarios navegan por tu aplicación.

Puedes llamar a SetTitleBar para cambiar a un nuevo elemento de barra de título mientras se ejecuta tu aplicación. También puedes pasar **null** como el parámetro de SetTitleBar para revertir al comportamiento de arrastre predeterminado. (Consulta "Región arrastrable predeterminada" para obtener más información).

> [!IMPORTANT]
> La región arrastrable que especifiques debe poder someterse a la prueba de posicionamiento lo que significa que, para algunos elementos, es posible que tengas que establecer un pincel de fondo transparente. Consulta los comentarios sobre [VisualTreeHelper.FindElementsInHostCoordinates](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates) para obtener más información.
>
>Por ejemplo, si defines una cuadrícula como la región arrastable, establece `Background="Transparent"` para que sea arrastrable.
>
>Esta cuadrícula no es arrastrable (pero sí lo son los elementos visibles que se encuentran dentro): `<Grid x:Name="AppTitleBar">`.
>
>Esta cuadrícula tiene el mismo aspecto, pero la cuadrícula completa es arrastable: `<Grid x:Name="AppTitleBar" Background="Transparent">`.

#### <a name="default-draggable-region"></a>Región arrastrable predeterminada

Si no especificas una región desplazable, un rectángulo que tenga el ancho de la ventana, el alto de los botones de subtítulo y esté colocado en el borde superior de la ventana se establece como la región arrastrable predeterminada.

Si defines una región arrastable, el sistema reduce el tamaño de la región arrastrable predeterminada a un área pequeña del tamaño de un botón de título, situado a la izquierda de los botones de título (o a la derecha si los botones de título se encuentran en el lado izquierdo de la ventana). Esto garantiza que siempre es un área coherente que el usuario puede arrastrar.

### <a name="system-caption-buttons"></a>Botones de título del sistema

El sistema reserva la esquina superior izquierda o superior derecha de la ventana de la aplicación para los botones de título del sistema (Atrás, Minimizar, Maximizar, Cerrar). El sistema mantiene el control del área de control de título para garantizar que se proporciona la funcionalidad mínima para arrastrar, minimizar, maximizar y cerrar la ventana. El sistema dibuja el botón Cerrar en la parte superior derecha para los idiomas que se escriben de izquierda a derecha y en la parte superior izquierda para los idiomas que se escriben de derecha a izquierda.

Las dimensiones y la posición del área de control de título se comunican mediante la clase CoreApplicationViewTitleBar, de modo que puede tenerlas en cuenta en el diseño de la interfaz de usuario de la barra del título. El ancho de la región reservada en cada lado lo proporcionan las propiedades [SystemOverlayLeftInset](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayLeftInset) o [SystemOverlayRightInset](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayRightInset) y su alto lo determina la propiedad [Height](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.Height).

Puedes dibujar contenido debajo de la superficie de control de subtítulo definida por estas propiedades, como el fondo de la aplicación, pero no debes colocar ninguna interfaz de usuario con la que esperes que el usuario puede interactuar. No recibe ninguna entrada porque la entrada para los controles de título se controlada por el sistema.

Puedes controlar el evento [LayoutMetricsChanged](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.LayoutMetricsChanged) para responder a cambios en el tamaño de los botones de título. Por ejemplo, esto puede suceder cuando se muestra o se oculta el botón Atrás del sistema. Controle este evento para comprobar y actualizar la posición de los elementos de la interfaz de usuario que dependen del tamaño de la barra de título.

En este ejemplo se muestra cómo ajustar el diseño de la barra de título para tener en cuenta cambios como el botón Atrás del sistema que se muestran u ocultan. `AppTitleBar`, `LeftPaddingColumn` y `RightPaddingColumn` se declaran en el código XAML que se ha mostrado anteriormente.

```csharp
private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    UpdateTitleBarLayout(sender);
}

private void UpdateTitleBarLayout(CoreApplicationViewTitleBar coreTitleBar)
{
    // Get the size of the caption controls area and back button 
    // (returned in logical pixels), and move your content around as necessary.
    LeftPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayLeftInset);
    RightPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayRightInset);
    TitleBarButton.Margin = new Thickness(0,0,coreTitleBar.SystemOverlayRightInset,0);

    // Update title bar control size as needed to account for system size changes.
    AppTitleBar.Height = coreTitleBar.Height;
}
```

### <a name="interactive-content"></a>Contenido interactivo

Puedes colocar controles interactivos, como botones, menús o un cuadro de búsqueda en la parte superior de la app para que aparezcan en la barra de título. Sin embargo, hay algunas reglas que debes seguir para asegurarte de que los elementos interactivos reciben la entrada de usuario.
- Debes llamar a SetTitleBar para definir un área como la región de la barra de título arrastrable. Si no lo haces, el sistema establece la región arrastrable predeterminada en la parte superior de la página. El sistema controlará entonces toda la entrada de usuario para esta área y evitará que la entrada llegue a los controles.
- Coloca los controles interactivos en la parte superior de la región arrastrable definida por la llamada en SetTitleBar (con un orden z superior). No conviertas tus controles interactivos en elementos secundarios del UIElement pasado a SetTitleBar. Cuando pases un elemento a SetTitleBar, el sistema lo trata como la barra de título del sistema y controla toda la entrada de puntero para ese elemento.

Aquí, el elemento `TitleBarButton` tiene un orden Z superior a `AppTitleBar`, de modo que recibe la entrada del usuario.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition />
    </Grid.RowDefinitions>

    <Grid x:Name="AppTitleBar" Background="Transparent">
        <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
        <!-- Using padding columns instead of Margin ensures that the background
             paints the area under the caption control buttons (for transparent buttons). -->
        <Grid.ColumnDefinitions>
            <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
            <ColumnDefinition/>
            <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
        </Grid.ColumnDefinitions>
        <Image Source="Assets/Square44x44Logo.png"
               Grid.Column="1" HorizontalAlignment="Left"
               Width="20" Height="20" Margin="12,0"/>
        <TextBlock Text="Custom Title Bar"
                   Grid.Column="1"
                   Style="{StaticResource CaptionTextBlockStyle}"
                   Margin="44,8,0,0"/>
    </Grid>

    <!-- This Button has a higher z-order than AppTitleBar, 
         so it receives user input. -->
    <Button x:Name="TitleBarButton" Content="Button in the title bar"
        HorizontalAlignment="Right"/>
</Grid>
```

### <a name="transparency-in-caption-buttons"></a>Transparencia en los botones de título

Al establecer ExtendViewIntoTitleBar en **true**, puedes hacer transparente el fondo de los botones de título para que se muestre el fondo de la aplicación. Normalmente, estableces el fondo en [Colors.Transparent](https://docs.microsoft.com/uwp/api/windows.ui.colors.Transparent) para lograr una transparencia completa. Para una transparencia parcial, establece el canal alfa para el valor de [Color](https://docs.microsoft.com/uwp/api/windows.ui.color) en el que estableces la propiedad.

Estas propiedades ApplicationViewTitleBar pueden ser transparentes:

- ButtonBackgroundColor
- ButtonHoverBackgroundColor
- ButtonPressedBackgroundColor
- ButtonInactiveBackgroundColor

El color de fondo del botón no se aplica a los de estados de presionado y mantener el mouse del botón Cerrar. El botón Cerrar siempre usa el color definido por el sistema para esos estados.

Todas las demás propiedades de color seguirán ignorando el canal alfa. Si ExtendViewIntoTitleBar se establece en **false**, siempre se ignora el canal alfa para todas las propiedades de color ApplicationViewTitleBar.

### <a name="full-screen-and-tablet-mode"></a>Pantalla completa y modo tableta

Cuando la app se ejecuta en _pantalla completa_ o _modo tableta_, el sistema oculta la barra de título y los botones de control de título. Sin embargo, el usuario puede invocar la barra de título para que se muestre como una superposición sobre la interfaz de usuario de la aplicación.
Puedes controlar el evento [CoreApplicationViewTitleBar.IsVisibleChanged](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.IsVisibleChanged) para recibir notificaciones cuando se oculte o se invoque la barra de título, o mostrar u ocultar el contenido de la barra de título personalizado según sea necesario.

Este ejemplo muestra cómo controlar IsVisibleChanged para mostrar y ocultar el elemento `AppTitleBar` mostrado anteriormente.

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;

    // Register a handler for when the title bar visibility changes.
    // For example, when the title bar is invoked in full screen mode.
    coreTitleBar.IsVisibleChanged += CoreTitleBar_IsVisibleChanged;
}

private void CoreTitleBar_IsVisibleChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (sender.IsVisible)
    {
        AppTitleBar.Visibility = Visibility.Visible;
    }
    else
    {
        AppTitleBar.Visibility = Visibility.Collapsed;
    }
}
```

>[!NOTE]
>El modo _pantalla completa_ solo se puede especificar si lo admite la app. Consulta [ApplicationView.IsFullScreenMode](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.IsFullScreenMode) para obtener más información. [_Modo tableta_](https://support.microsoft.com/help/17210/windows-10-use-your-pc-like-a-tablet) es una opción de usuario en hardware compatible, por lo que un usuario puede optar por ejecutar cualquier app en el modo tableta.

## <a name="full-customization-example"></a>Ejemplo de personalización completa

```xaml
<Page
    x:Class="CustomTitleBar.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:CustomTitleBar"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition />
        </Grid.RowDefinitions>

        <Grid x:Name="AppTitleBar" Background="Transparent">
            <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
            <!-- Using padding columns instead of Margin ensures that the background
                 paints the area under the caption control buttons (for transparent buttons). -->
            <Grid.ColumnDefinitions>
                <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
                <ColumnDefinition/>
                <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
            </Grid.ColumnDefinitions>
            <Image Source="Assets/Square44x44Logo.png" 
                   Grid.Column="1" HorizontalAlignment="Left" 
                   Width="20" Height="20" Margin="12,0"/>
            <TextBlock Text="Custom Title Bar" 
                       Grid.Column="1" 
                       Style="{StaticResource CaptionTextBlockStyle}" 
                       Margin="44,8,0,0"/>
        </Grid>

        <!-- This Button has a higher z-order than MyTitleBar, 
             so it receives user input. -->
        <Button x:Name="TitleBarButton" Content="Button in the title bar"
                HorizontalAlignment="Right"/>
    </Grid>
</Page>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Hide default title bar.
    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;
    UpdateTitleBarLayout(coreTitleBar);

    // Set XAML element as a draggable region.
    Window.Current.SetTitleBar(AppTitleBar);

    // Register a handler for when the size of the overlaid caption control changes.
    // For example, when the app moves to a screen with a different DPI.
    coreTitleBar.LayoutMetricsChanged += CoreTitleBar_LayoutMetricsChanged;

    // Register a handler for when the title bar visibility changes.
    // For example, when the title bar is invoked in full screen mode.
    coreTitleBar.IsVisibleChanged += CoreTitleBar_IsVisibleChanged;
}

private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    UpdateTitleBarLayout(sender);
}

private void UpdateTitleBarLayout(CoreApplicationViewTitleBar coreTitleBar)
{
    // Get the size of the caption controls area and back button 
    // (returned in logical pixels), and move your content around as necessary.
    LeftPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayLeftInset);
    RightPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayRightInset);
    TitleBarButton.Margin = new Thickness(0,0,coreTitleBar.SystemOverlayRightInset,0);

    // Update title bar control size as needed to account for system size changes.
    AppTitleBar.Height = coreTitleBar.Height;
}

private void CoreTitleBar_IsVisibleChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (sender.IsVisible)
    {
        AppTitleBar.Visibility = Visibility.Visible;
    }
    else
    {
        AppTitleBar.Visibility = Visibility.Collapsed;
    }
}
```

## <a name="dos-and-donts"></a>Lo que se debe y no se debe hacer

- Lo que se debe hacer es obvio cuando la ventana está activa o inactiva. Como mínimo, cambia el color del texto, los iconos y los botones en la barra de título.
- Define una región arrastrable en el borde superior del lienzo de la app. La coincidencia con la colocación de las barras de título del sistema facilita que los usuarios la encuentren.
- Define una región arrastrable que coincida con la barra de título visual (si existe) en el lienzo de la aplicación.

## <a name="related-articles"></a>Artículos relacionados

- [Acrylic](../style/acrylic.md)
- [Color](../style/color.md)
