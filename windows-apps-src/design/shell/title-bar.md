---
description: Personalice la barra de título de una aplicación de escritorio para que coincida con la personalidad de la aplicación.
title: Personalización de la barra de título
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, barra de título
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 9d8aa92ec320c18b1947cb9b3fa7777070e19726
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220088"
---
# <a name="title-bar-customization"></a>Personalización de la barra de título



Cuando la aplicación se ejecuta en una ventana de escritorio, puede personalizar las barras de título para que coincidan con la personalidad de la aplicación. Las API de personalización de la barra de título permiten especificar los colores de los elementos de la barra de título, o ampliar el contenido de la aplicación en el área de la barra de título y tomar el control total del mismo.

> **API importantes**: [propiedad ApplicationView. TitleBar](/uwp/api/windows.ui.viewmanagement.applicationview), [clase ApplicationViewTitleBar](/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar), [clase CoreApplicationViewTitleBar](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar)

## <a name="how-much-to-customize-the-title-bar"></a>La cantidad de personalización de la barra de título

Hay dos niveles de personalización que se pueden aplicar a la barra de título.

Para la personalización de color simple, puede establecer las propiedades de [ApplicationViewTitleBar](/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) para especificar los colores que desea utilizar para los elementos de la barra de título. En este caso, el sistema conserva la responsabilidad de todos los demás aspectos de la barra de título, como dibujar el título de la aplicación y definir áreas arrastrables.

La otra opción consiste en ocultar la barra de título predeterminada y reemplazarla por su propio contenido XAML. Por ejemplo, puede colocar texto, botones o menús personalizados en el área de la barra de título. También tendrá que usar esta opción para extender un fondo [acrílico](../style/acrylic.md) en el área de la barra de título.

Al optar por la personalización completa, usted es responsable de colocar el contenido en el área de la barra de título y puede definir su propia región de arrastre. Los botones atrás, cerrar, minimizar y maximizar del sistema siguen estando disponibles y administrados por el sistema, pero no los elementos como el título de la aplicación. Tendrá que crear esos elementos según sea necesario para la aplicación.

> [!NOTE]
> La personalización de color simple está disponible para las aplicaciones de Windows mediante XAML, DirectX y HTML. La personalización completa solo está disponible para las aplicaciones de Windows que usan XAML.

## <a name="simple-color-customization"></a>Personalización de color simple

Si solo desea personalizar los colores de la barra de título y no hacer nada más sofisticado (por ejemplo, colocar las pestañas en la barra de título), puede establecer las propiedades de color en el [ApplicationViewTitleBar](/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) de la ventana de la aplicación.

En este ejemplo se muestra cómo obtener una instancia de ApplicationViewTitleBar y establecer sus propiedades de color.

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
> Este código se puede colocar en el método [onlaunched](/uwp/api/windows.ui.xaml.application.onlaunched) de la aplicación (_app.Xaml.CS_), después de la llamada a [window. Activate](/uwp/api/windows.ui.xaml.window.Activate)o en la primera página de la aplicación.

> [!TIP]
> El kit de herramientas de la comunidad de Windows proporciona extensiones que permiten establecer estas propiedades de color en XAML. Para obtener más información, consulte la [documentación del kit de herramientas](/windows/uwpcommunitytoolkit/extensions/viewextensions)de la comunidad de Windows.

Hay algunos aspectos que se deben tener en cuenta al establecer los colores de la barra de título:

- El color de fondo del botón no se aplica al estado de mantener presionado del botón Cerrar. El botón Cerrar siempre usa el color definido por el sistema para esos Estados.
- Las propiedades de color del botón se aplican al botón atrás del sistema cuando se usa. ([Consulte el historial de navegación y la navegación hacia atrás](../basics/navigation-history-and-backwards-navigation.md)).
- Al establecer una propiedad de color en **null** , se restablece el color predeterminado del sistema.
- No se pueden establecer colores transparentes. Se omite el canal alfa del color.

Windows ofrece a los usuarios la opción de aplicar el [color de énfasis](../style/color.md#accent-color) seleccionado a la barra de título. Si establece un color de barra de título, se recomienda establecer explícitamente todos los colores. Esto garantiza que no hay combinaciones de colores no deseadas que se produzcan debido a la configuración de color definida por el usuario.

## <a name="full-customization"></a>Personalización completa

Al participar en la personalización de la barra de título completa, el área cliente de la aplicación se amplía para abarcar toda la ventana, incluido el área de la barra de título. El usuario es responsable del dibujo y del control de entrada de toda la ventana excepto de los botones de título, que están superpuestos en la parte superior del lienzo de la aplicación.

Para ocultar la barra de título predeterminada y extender el contenido en el área de la barra de título, establezca la propiedad [CoreApplicationViewTitleBar. ExtendViewIntoTitleBar](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar) en **true**.

En este ejemplo se muestra cómo obtener CoreApplicationViewTitleBar y establecer la propiedad ExtendViewIntoTitleBar en **true**. Esto puede hacerse en el método [onlaunched](/uwp/api/windows.ui.xaml.application.onlaunched) de la aplicación (_app.Xaml.CS_) o en la primera página de la aplicación.

```csharp
// using Windows.ApplicationModel.Core;

// Hide default title bar.
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
coreTitleBar.ExtendViewIntoTitleBar = true;
```

> [!TIP]
> Esta configuración se conserva cuando la aplicación se cierra y se reinicia. En Visual Studio, si establece ExtendViewIntoTitleBar en **true**y después desea volver al valor predeterminado, debe establecerlo explícitamente en **false** y ejecutar la aplicación para sobrescribir la configuración persistente.

### <a name="draggable-regions"></a>Regiones arrastrables

La región arrastrable de la barra de título define el lugar en el que el usuario puede hacer clic y arrastrar para mover la ventana (en lugar de arrastrar simplemente el contenido dentro del lienzo de la aplicación). Especifique la región arrastrable llamando al método [window. SetTitleBar](/uwp/api/windows.ui.xaml.window.settitlebar) y pasando un UIElement que defina la región arrastrable. (El UIElement suele ser un panel que contiene otros elementos).

Aquí se muestra cómo establecer una cuadrícula de contenido como la región de la barra de título que se va a arrastrar. Este código va en el código XAML y en el código subyacente de la primera página de la aplicación. Vea la sección [ejemplo de personalización completa](./title-bar.md#full-customization-example) para ver el código completo.


> [!IMPORTANT]
> De forma predeterminada, algunos elementos de la interfaz de usuario, como Grid, no participan en la prueba de posicionamiento cuando no tienen un conjunto de fondo.
> En la cuadrícula del `AppTitleBar` ejemplo siguiente para permitir el arrastre, por lo tanto, es necesario establecer el fondo en `Transparent` .

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
    coreTitleBar.LayoutMetricsChanged += CoreTitleBar_LayoutMetricsChanged;
    // Set XAML element as a draggable region.
    Window.Current.SetTitleBar(AppTitleBar);
}

private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    AppTitleBar.Height = sender.Height;
}
```

UIElement ( `AppTitleBar` ) forma parte del XAML de la aplicación. Puede declarar y establecer la barra de título en una página raíz que no cambie, o bien declarar y establecer un área de la barra de título en cada página a la que pueda navegar su aplicación. Si lo establece en cada página, debe asegurarse de que la región arrastrable permanece coherente cuando un usuario navega por la aplicación.

Puede llamar a SetTitleBar para cambiar a un nuevo elemento de la barra de título mientras la aplicación se está ejecutando. También puede pasar **null** como parámetro a SetTitleBar para revertir al comportamiento de arrastre predeterminado. (Consulte "región de arrastre predeterminada" para obtener más información).

> [!IMPORTANT]
> La región que se puede arrastrar que especifique debe ser una prueba de posicionamiento, lo que significa que, para algunos elementos, es posible que tenga que establecer un pincel de fondo transparente. Vea los comentarios en [VisualTreeHelper. FindElementsInHostCoordinates](/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates) para obtener más información.
>
>Por ejemplo, si define una cuadrícula como su región de arrastre, establezca `Background="Transparent"` para que se pueda arrastrar.
>
>Esta cuadrícula no es arrastrable (pero los elementos visibles dentro de ella son): `<Grid x:Name="AppTitleBar">` .
>
>Esta cuadrícula tiene el mismo aspecto, pero se arrastra toda la cuadrícula: `<Grid x:Name="AppTitleBar" Background="Transparent">` .

#### <a name="default-draggable-region"></a>Región de arrastre predeterminada

Si no se especifica una región arrastrable, un rectángulo que sea el ancho de la ventana, el alto de los botones de título y colocado a lo largo del borde superior de la ventana se establecerá como la región de arrastre predeterminada.

Si define una región arrastrable, el sistema reduce la región arrastrable predeterminada hacia abajo hasta un área pequeña, el tamaño de un botón de título, situado a la izquierda de los botones de título (o a la derecha si los botones de título están en el lado izquierdo de la ventana). Esto garantiza que siempre haya un área coherente que el usuario pueda arrastrar.

### <a name="system-caption-buttons"></a>Botones de título del sistema

El sistema reserva la esquina superior izquierda o superior derecha de la ventana de la aplicación para los botones de título del sistema (atrás, minimizar, maximizar y cerrar). El sistema conserva el control del área de control de títulos para garantizar que se proporciona la funcionalidad mínima para arrastrar, minimizar, maximizar y cerrar la ventana. El sistema dibuja el botón cerrar en la parte superior derecha para los idiomas de izquierda a derecha y en la parte superior izquierda para los idiomas que se van de derecha a izquierda.

La clase CoreApplicationViewTitleBar comunica las dimensiones y la posición del área de control Caption para que se pueda tener en cuenta en el diseño de la interfaz de usuario de la barra de título. El ancho de la región reservada en cada lado lo proporcionan las propiedades [SystemOverlayLeftInset](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayLeftInset) o [SystemOverlayRightInset](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayRightInset) , y el alto lo proporciona la propiedad [height](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.Height) .

Puede dibujar contenido debajo del área de control de título definida por estas propiedades, como el fondo de la aplicación, pero no debe colocar ninguna interfaz de usuario con la que espera que el usuario pueda interactuar. No recibe ninguna entrada porque el sistema controla la entrada para los controles de título.

Puede controlar el evento [LayoutMetricsChanged](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.LayoutMetricsChanged) para responder a los cambios en el tamaño de los botones de título. Por ejemplo, esto puede ocurrir cuando se muestra u oculta el botón atrás del sistema. Controle este evento para comprobar y actualizar la posición de los elementos de la interfaz de usuario que dependen del tamaño de la barra de título.

En este ejemplo se muestra cómo ajustar el diseño de la barra de título para tener en cuenta los cambios como el botón atrás del sistema que se muestra u oculta. `AppTitleBar`, `LeftPaddingColumn` y `RightPaddingColumn` se declaran en el código XAML mostrado previamente.

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

Puede colocar controles interactivos, como botones, menús o un cuadro de búsqueda, en la parte superior de la aplicación para que aparezcan en la barra de título. Sin embargo, hay algunas reglas que debe seguir para asegurarse de que los elementos interactivos reciban datos proporcionados por el usuario.
- Debe llamar a SetTitleBar para definir un área como la región de la barra de título que se puede arrastrar. Si no lo hace, el sistema establece la región de arrastre predeterminada en la parte superior de la página. A continuación, el sistema administrará todos los datos proporcionados por el usuario en esta área e impedirá que los datos lleguen a los controles.
- Coloque los controles interactivos en la parte superior de la región arrastrable definida por la llamada a SetTitleBar (con un orden z superior). No convierta los controles interactivos en los elementos secundarios del UIElement pasado a SetTitleBar. Después de pasar un elemento a SetTitleBar, el sistema lo trata como la barra de título del sistema y controla toda la entrada de puntero a ese elemento.

Aquí, el `TitleBarButton` elemento tiene un orden z superior a `AppTitleBar` , por lo que recibe la entrada del usuario.

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

Si establece ExtendViewIntoTitleBar en **true**, puede hacer que el fondo de los botones de título sea transparente para que el fondo de la aplicación se muestre. Normalmente se establece el fondo en [colors. Transparent](/uwp/api/windows.ui.colors.Transparent) para una transparencia completa. Para una transparencia parcial, establezca el canal alfa del [color](/uwp/api/windows.ui.color) en el que establece la propiedad.

Estas propiedades de ApplicationViewTitleBar pueden ser transparentes:

- ButtonBackgroundColor
- ButtonHoverBackgroundColor
- ButtonPressedBackgroundColor
- ButtonInactiveBackgroundColor

El color de fondo del botón no se aplica al estado de mantener presionado del botón Cerrar. El botón Cerrar siempre usa el color definido por el sistema para esos Estados.

Todas las demás propiedades de color seguirán ignorando el canal alfa. Si ExtendViewIntoTitleBar se establece en **false**, el canal alfa siempre se omite para todas las propiedades de color ApplicationViewTitleBar.

### <a name="full-screen-and-tablet-mode"></a>Modo de pantalla completa y tableta

Cuando la aplicación se ejecuta en el modo de _pantalla completa_ o de _Tablet PC_, el sistema oculta los botones de control de la barra de título y del título. Sin embargo, el usuario puede invocar la barra de título para que se muestre como una superposición encima de la interfaz de usuario de la aplicación.
Puede controlar el evento [CoreApplicationViewTitleBar. IsVisibleChanged](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.IsVisibleChanged) para recibir una notificación cuando la barra de título se oculte o se invoque, y mostrar u ocultar el contenido de la barra de título personalizada según sea necesario.

En este ejemplo se muestra cómo controlar IsVisibleChanged para mostrar y ocultar el `AppTitleBar` elemento mostrado previamente.

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
>El modo de _pantalla completa_ solo se puede especificar si la aplicación lo admite. Vea [ApplicationView. IsFullScreenMode](/uwp/api/windows.ui.viewmanagement.applicationview.IsFullScreenMode) para obtener más información. El [_modo tableta_](https://support.microsoft.com/help/17210/windows-10-use-your-pc-like-a-tablet) es una opción de usuario en el hardware compatible, por lo que un usuario puede elegir ejecutar cualquier aplicación en modo de tableta.

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

## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar

- Haga que sea obvio cuando la ventana esté activa o inactiva. Como mínimo, cambie el color del texto, los iconos y los botones de la barra de título.
- Defina una región arrastrable a lo largo del borde superior del lienzo de la aplicación. La coincidencia de la ubicación de las barras de título del sistema facilita la búsqueda de los usuarios.
- Defina una región arrastrable que coincida con la barra de título visual (si existe) en el lienzo de la aplicación.

## <a name="related-articles"></a>Artículos relacionados

- [Acrílico](../style/acrylic.md)
- [Color](../style/color.md)
