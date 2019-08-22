---
Description: Use la clase AppWindow para ver diferentes partes de la aplicación en ventanas independientes.
title: Usar la clase AppWindow para mostrar las ventanas secundarias de una aplicación
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9b89d9100157cf40266bb983e258aa187f65dc93
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867461"
---
# <a name="show-multiple-views-with-appwindow"></a>Mostrar varias vistas con AppWindow

[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) y sus API relacionadas simplifican la creación de aplicaciones de varias ventanas, ya que permiten mostrar el contenido de la aplicación en ventanas secundarias mientras se sigue trabajando en el mismo subproceso de interfaz de usuario en cada ventana.

> [!NOTE]
> AppWindow se encuentra actualmente en versión preliminar. Esto significa que puede enviar aplicaciones que usan AppWindow a la tienda, pero se sabe que algunos componentes de plataforma y marco no funcionan con AppWindow (vea [limitaciones](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)).

Aquí se muestran algunos escenarios para varias ventanas con una aplicación de ejemplo denominada `HelloAppWindow`. En la aplicación de ejemplo se muestra la siguiente funcionalidad:

- Quitar el acoplamiento de un control de la Página principal y abrirlo en una nueva ventana.
- Abra nuevas instancias de una página en nuevas ventanas.
- Cambiar mediante programación el tamaño y la posición de las ventanas nuevas en la aplicación.
- Asociar un ContentDialog con la ventana correspondiente en la aplicación.

![Aplicación de ejemplo con una sola ventana](images/hello-app-window-single.png)
  
> _Aplicación de ejemplo con una sola ventana_

![Aplicación de ejemplo con un selector de colores desacoplado y una ventana secundaria](images/hello-app-window-multi.png)

> _Aplicación de ejemplo con un selector de colores desacoplado y una ventana secundaria_

> **API importantes**: [Espacio de nombres Windows. UI. WindowManagement](/uwp/api/windows.ui.windowmanagement), [clase AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)

## <a name="api-overview"></a>Introducción a API

La clase [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) y otras API del espacio de nombres [WindowManagement](/uwp/api/windows.ui.windowmanagement) están disponibles a partir de Windows 10, versión 1903 (SDK 18362). Si su aplicación tiene como destino versiones anteriores de Windows 10, debe [usar ApplicationView para crear ventanas secundarias](application-view.md). Las API de WindowManagement todavía están en desarrollo y tienen [limitaciones](/uwp/api/windows.ui.windowmanagement.appwindow#limitations) , como se describe en los documentos de referencia de la API.

Estas son algunas de las API importantes que se usan para mostrar el contenido en un AppWindow.

### <a name="appwindow"></a>AppWindow

La clase [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) se puede usar para mostrar una parte de una aplicación Windows Runtime en una ventana secundaria. Es similar en concepto a un [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview), pero no al mismo comportamiento y duración. Una característica principal de AppWindow es que cada instancia comparte el mismo subproceso de procesamiento de la interfaz de usuario (incluido el distribuidor de eventos) desde el que se crearon, lo que simplifica las aplicaciones de varias ventanas.

Solo puede conectar contenido XAML a su AppWindow, no se admite el contenido de DirectX o Holographic nativo. Sin embargo, puede mostrar un [SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel) de XAML que hospede contenido de DirectX.

### <a name="windowingenvironment"></a>WindowingEnvironment

La API de [WindowingEnvironment](/uwp/api/windows.ui.windowmanagement.windowingenvironment) le permite conocer el entorno en el que se presenta la aplicación para que pueda adaptar la aplicación según sea necesario. Describe el tipo de ventana que admite el entorno; por ejemplo, `Overlapped` si la aplicación se ejecuta en un equipo o `Tiled` si la aplicación se ejecuta en una consola Xbox. También proporciona un conjunto de objetos DisplayRegion que describen las áreas en las que una aplicación puede mostrarse en una pantalla lógica.

### <a name="displayregion"></a>DisplayRegion

La API [DisplayRegion](/uwp/api/windows.ui.windowmanagement.displayregion) describe la región en la que se puede mostrar una vista a un usuario en una pantalla lógica; por ejemplo, en un equipo de escritorio, se trata de la pantalla completa menos el área de la barra de tareas. No es necesariamente una asignación 1:1 con el área de visualización física del monitor de respaldo. Puede haber varias regiones de visualización en el mismo monitor, o bien se puede configurar un DisplayRegion para que abarque varios monitores si esos monitores son homogéneos en todos los aspectos.

### <a name="appwindowpresenter"></a>AppWindowPresenter

La API de [AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter) permite cambiar fácilmente las ventanas a configuraciones predefinidas `FullScreen` como `CompactOverlay`o. Estas configuraciones proporcionan a los usuarios una experiencia coherente en cualquier dispositivo que admita la configuración.

### <a name="uicontext"></a>UIContext

[Solutionopening](/uwp/api/windows.ui.uicontext) es un identificador único de una ventana o vista de la aplicación. Se crea automáticamente y puede usar la propiedad [UIElement. solutionopening](/uwp/api/windows.ui.xaml.uielement.uicontext) para recuperar el solutionopening. Cada UIElement en el árbol XAML tiene el mismo Solutionopening.

 Solutionopening es importante porque las API como [window. Current](/uwp/api/Windows.UI.Xaml.Window.Current) y `GetForCurrentView` el patrón dependen de tener un único ApplicationView/CoreWindow con un solo árbol XAML por subproceso con el que trabajar. Este no es el caso cuando se usa un AppWindow, por lo que se usa Solutionopening para identificar una ventana concreta en su lugar.

### <a name="xamlroot"></a>XamlRoot

La clase [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) contiene un árbol de elementos XAML, lo conecta al objeto de host de ventana (por ejemplo, [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) o [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)) y proporciona información como el tamaño y la visibilidad. No cree directamente un objeto XamlRoot. En su lugar, se crea una al adjuntar un elemento XAML a un AppWindow. Después, puede usar la propiedad [UIElement. XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) para recuperar el XamlRoot.

Para obtener más información sobre Solutionopening y XamlRoot, vea [crear código portable entre hosts de ventanas](show-multiple-views.md#make-code-portable-across-windowing-hosts).

## <a name="show-a-new-window"></a>Mostrar una nueva ventana

Echemos un vistazo a los pasos para mostrar el contenido en un nuevo AppWindow.

**Para mostrar una nueva ventana**

1. Llame al método estático [AppWindow. TryCreateAsync](/uwp/api/windows.ui.windowmanagement.appwindow.trycreateasync) para crear un nuevo [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow).

    ```csharp
    AppWindow appWindow = await AppWindow.TryCreateAsync();
    ```

1. Cree el contenido de la ventana.

    Normalmente, se crea un [marco](/uwp/api/Windows.UI.Xaml.Controls.Frame)XAML y, a continuación, se desplaza el marco a una [Página](/uwp/api/Windows.UI.Xaml.Controls.Page) XAML donde se ha definido el contenido de la aplicación. Para obtener más información sobre los marcos y las páginas, consulte [navegación punto a punto entre dos páginas](../basics/navigate-between-two-pages.md).

    ```csharp
    Frame appWindowContentFrame = new Frame();
    appWindowContentFrame.Navigate(typeof(AppWindowMainPage));
    ```

    Sin embargo, puede mostrar cualquier contenido XAML en el AppWindow, no solo un marco y una página. Por ejemplo, puede mostrar un solo control, como [ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker), o puede mostrar un [SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel) que hospede contenido de DirectX.

1. Llame al método [ElementCompositionPreview. SetAppWindowContent](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent) para adjuntar el contenido XAML a AppWindow.

    ```csharp
    ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);
    ```

    La llamada a este método crea un objeto [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) y lo establece como la propiedad [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) para el UIElement especificado.

    Solo puede llamar a este método una vez por cada instancia de AppWindow. Una vez establecido el contenido, se producirá un error en otras llamadas a SetAppWindowContent para esta instancia de AppWindow. Además, si intenta desconectar el contenido de AppWindow pasando un objeto UIElement nulo, se producirá un error en la llamada.

1. Llame al método [AppWindow. TryShowAsync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync) para mostrar la ventana nueva.

    ```csharp
    await appWindow.TryShowAsync();
    ```

## <a name="release-resources-when-a-window-is-closed"></a>Liberar recursos cuando se cierra una ventana

Siempre debe controlar el evento [AppWindow. Closed](/uwp/api/windows.ui.windowmanagement.appwindow.closed) para liberar los recursos XAML (el contenido de AppWindow) y las referencias a AppWindow.

```csharp
appWindow.Closed += delegate
{
    appWindowContentFrame.Content = null;
    appWindow = null;
};
```

## <a name="track-instances-of-appwindow"></a>Seguimiento de instancias de AppWindow

En función de cómo use varias ventanas en la aplicación, puede que necesite o no realizar un seguimiento de las instancias de AppWindow que cree. En `HelloAppWindow` el ejemplo se muestran algunas maneras diferentes en las que normalmente se usa un [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow). Aquí veremos por qué se debe realizar el seguimiento de estas ventanas y cómo hacerlo.

### <a name="simple-tracking"></a>Seguimiento simple

La ventana selector de colores hospeda un único control XAML y el código para interactuar con el selector de colores reside en el `MainPage.xaml.cs` archivo. La ventana selector de colores solo permite una instancia única y es esencialmente una extensión de `MainWindow`. Para asegurarse de que solo se crea una instancia, se realiza un seguimiento de la ventana del selector de colores con una variable de nivel de página. Antes de crear una nueva ventana del selector de colores, compruebe si existe una instancia y, en caso de que sea así, omita los pasos para crear una nueva ventana y simplemente llame a [TryShowAsync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync) en la ventana existente.

```csharp
AppWindow colorPickerAppWindow;

// ...

private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
{
    // Create the color picker window.
    if (colorPickerAppWindow == null)
    {
        // ...
        // Create a new window
        colorPickerAppWindow = await AppWindow.TryCreateAsync();
        // ...
    }
    // Show the window.
    await colorPickerAppWindow.TryShowAsync();
}
```

### <a name="track-an-appwindow-instance-in-its-hosted-content"></a>Seguimiento de una instancia de AppWindow en su contenido hospedado

La `AppWindowPage` ventana hospeda una página XAML completa y el código para interactuar con la página reside en `AppWindowPage.xaml.cs`. Permite varias instancias, cada una de las cuales funciona de forma independiente.

La funcionalidad de la página le permite manipular la ventana, establecerla en `FullScreen` o `CompactOverlay`y también escucha los eventos [AppWindow. Changed](/uwp/api/windows.ui.windowmanagement.appwindow.changed) para mostrar información sobre la ventana. Para poder llamar a estas API, `AppWindowPage` necesita una referencia a la instancia de AppWindow que la hospeda.

Si eso es todo lo que necesita, puede crear una propiedad en `AppWindowPage` y asignarle la instancia de [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) al crearla.

**AppWindowPage.xaml.cs**

En `AppWindowPage`, cree una propiedad que contenga la referencia de AppWindow.

```csharp
public sealed partial class AppWindowPage : Page
{
    public AppWindow MyAppWindow { get; set; }

    // ...
}
```

**MainPage.xaml.cs**

En `MainPage`, obtenga una referencia a la instancia de la página y asigne el AppWindow recién creado a la `AppWindowPage`propiedad en.

```csharp
private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
{
    // Create a new window.
    AppWindow appWindow = await AppWindow.TryCreateAsync();

    // Create a Frame and navigate to the Page you want to show in the new window.
    Frame appWindowContentFrame = new Frame();
    appWindowContentFrame.Navigate(typeof(AppWindowPage));

    // Get a reference to the page instance and assign the
    // newly created AppWindow to the MyAppWindow property.
    AppWindowPage page = (AppWindowPage)appWindowContentFrame.Content;
    page.MyAppWindow = appWindow;

    // ...
}
```

### <a name="tracking-app-windows-using-uicontext"></a>Seguimiento de las ventanas de la aplicación mediante Solutionopening

También puede que desee tener acceso a las instancias de [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) desde otras partes de la aplicación. Por ejemplo, `MainPage` podría tener un botón ' cerrar todo ' que cierre todas las instancias de las que se ha realizado un seguimiento de AppWindow.

En este caso, debe usar el identificador único de [solutionopening](/uwp/api/windows.ui.uicontext) para realizar el seguimiento de las instancias de ventana en un [Diccionario](/dotnet/api/system.collections.generic.dictionary-2?view=dotnet-uwp-10.0).

**MainPage.xaml.cs**

En `MainPage`, cree el diccionario como una propiedad estática. A continuación, agregue la página al diccionario al crearla y quítela cuando se cierre la página. Puede obtener el solutionopening desde el [marco](/uwp/api/Windows.UI.Xaml.Controls.Frame) de contenido (`appWindowContentFrame.UIContext`) después de llamar a [ElementCompositionPreview. SetAppWindowContent](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent).

```csharp
public sealed partial class MainPage : Page
{
    // Track open app windows in a Dictionary.
    public static Dictionary<UIContext, AppWindow> AppWindows { get; set; }
        = new Dictionary<UIContext, AppWindow>();

    // ...

    private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
    {
        // Create a new window.
        AppWindow appWindow = await AppWindow.TryCreateAsync();

        // Create a Frame and navigate to the Page you want to show in the new window.
        Frame appWindowContentFrame = new Frame();
        appWindowContentFrame.Navigate(typeof(AppWindowPage));

        // Attach the XAML content to the window.
        ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);

        // Add the new page to the Dictionary using the UIContext as the Key.
        AppWindows.Add(appWindowContentFrame.UIContext, appWindow);
        appWindow.Title = "App Window " + AppWindows.Count.ToString();

        // When the window is closed, be sure to release
        // XAML resources and the reference to the window.
        appWindow.Closed += delegate
        {
            MainPage.AppWindows.Remove(appWindowContentFrame.UIContext);
            appWindowContentFrame.Content = null;
            appWindow = null;
        };

        // Show the window.
        await appWindow.TryShowAsync();
    }

    private async void CloseAllButton_Click(object sender, RoutedEventArgs e)
    {
        while (AppWindows.Count > 0)
        {
            await AppWindows.Values.First().CloseAsync();
        }
    }
    // ...
}
```

**AppWindowPage.xaml.cs**

Para usar la instancia de [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) en `AppWindowPage` el código, use el [solutionopening](/uwp/api/windows.ui.uicontext) de la página para recuperarlo del diccionario estático `MainPage`en. Debe hacer esto en el controlador de eventos [Loaded](/uwp/api/windows.ui.xaml.frameworkelement.loaded) de la página en lugar de en el constructor para que solutionopening no sea NULL. Puede obtener el Solutionopening de la página: `this.UIContext`.

```csharp
public sealed partial class AppWindowPage : Page
{
    AppWindow window;

    // ...
    public AppWindowPage()
    {
        this.InitializeComponent();

        Loaded += AppWindowPage_Loaded;
    }

    private void AppWindowPage_Loaded(object sender, RoutedEventArgs e)
    {
        // Get the reference to this AppWindow that was stored when it was created.
        window = MainPage.AppWindows[this.UIContext];

        // Set up event handlers for the window.
        window.Changed += Window_Changed;
    }
    // ...
}
```

> [!NOTE]
> En `HelloAppWindow` el ejemplo se muestran las dos maneras de realizar `AppWindowPage`el seguimiento de la ventana en, pero normalmente se usa uno u otro, no ambos.

## <a name="request-window-size-and-placement"></a>Tamaño y ubicación de la ventana de solicitud

La clase [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) tiene varios métodos que se pueden utilizar para controlar el tamaño y la posición de la ventana. Como se trata de los nombres de método, el sistema puede cumplir o no los cambios solicitados en función de los factores del entorno.

Llame a la [solicitud](/uwp/api/windows.ui.windowmanagement.appwindow.requestsize) para especificar el tamaño de ventana deseado, como este.

```csharp
colorPickerAppWindow.RequestSize(new Size(300, 428));
```

Los métodos para administrar la ubicación de las ventanas se denominan _RequestMove *_ : [RequestMoveAdjacentToCurrentView](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttocurrentview), [RequestMoveAdjacentToWindow](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttowindow), [RequestMoveRelativeToDisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoverelativetodisplayregion), [RequestMoveToDisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmovetodisplayregion).

En este ejemplo, este código mueve la ventana para que esté junto a la vista principal de la que se genera la ventana.

```csharp
colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
```

Para obtener información sobre el tamaño actual y la posición de la ventana, llame a [GetPlacement](/uwp/api/windows.ui.windowmanagement.appwindow.getplacement). Esto devuelve un objeto [AppWindowPlacement](/uwp/api/windows.ui.windowmanagement.appwindowplacement) que proporciona el [DisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindowplacement.displayregion)actual, el [desplazamiento](/uwp/api/windows.ui.windowmanagement.appwindowplacement.offset)y el [tamaño](/uwp/api/windows.ui.windowmanagement.appwindowplacement.size) de la ventana.

Por ejemplo, podría llamar a este código para colocar la ventana en la esquina superior derecha de la pantalla. Se debe llamar a este código después de que se muestre la ventana; de lo contrario, el tamaño de la ventana devuelto por la llamada a GetPlacement será 0, 0 y el desplazamiento será incorrecto.

```csharp
DisplayRegion displayRegion = window.GetPlacement().DisplayRegion;
double displayRegionWidth = displayRegion.WorkAreaSize.Width;
double windowWidth = window.GetPlacement().Size.Width;
int horizontalOffset = (int)(displayRegionWidth - windowWidth);
window.RequestMoveRelativeToDisplayRegion(displayRegion, new Point(horizontalOffset, 0));
```

## <a name="request-a-presentation-configuration"></a>Solicitar una configuración de presentación

La clase [AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter) permite mostrar un [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) con una configuración predefinida adecuada para el dispositivo en el que se muestra. Puede usar un valor de [AppWindowPresentationConfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresentationconfiguration) para colocar la ventana en `FullScreen` el `CompactOverlay` modo o.

En este ejemplo se muestra cómo hacer lo siguiente:

- Use el evento [AppWindow. Changed](/uwp/api/windows.ui.windowmanagement.appwindow.changed) para recibir una notificación si cambian las presentaciones de ventana disponibles.
- Use la propiedad [AppWindow. Presenter](/uwp/api/windows.ui.windowmanagement.appwindow.presenter) para obtener la [AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter)actual.
- Llame a [IsPresentationSupported](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.ispresentationsupported) para ver si se admite un [AppWindowPresentationKind](/uwp/api/windows.ui.windowmanagement.appwindowpresentationkind) específico.
- Llame a [GetConfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.getconfiguration) para comprobar qué tipo de configuración se usa actualmente.
- Llame a [RequestPresentation](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.requestpresentation) para cambiar la configuración actual.

```csharp
private void Window_Changed(AppWindow sender, AppWindowChangedEventArgs args)
{
    if (args.DidAvailableWindowPresentationsChange)
    {
        EnablePresentationButtons(sender);
    }

    if (args.DidWindowPresentationChange)
    {
        ConfigText.Text = window.Presenter.GetConfiguration().Kind.ToString();
    }

    if (args.DidSizeChange)
    {
        SizeText.Text = window.GetPlacement().Size.ToString();
    }
}

private void EnablePresentationButtons(AppWindow window)
{
    // Check whether the current AppWindowPresenter supports CompactOverlay.
    if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.CompactOverlay))
    {
        // Show the CompactOverlay button...
        compactOverlayButton.Visibility = Visibility.Visible;
    }
    else
    {
        // Hide the CompactOverlay button...
        compactOverlayButton.Visibility = Visibility.Collapsed;
    }

    // Check whether the current AppWindowPresenter supports FullScreen?
    if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.FullScreen))
    {
        // Show the FullScreen button...
        fullScreenButton.Visibility = Visibility.Visible;
    }
    else
    {
        // Hide the FullScreen button...
        fullScreenButton.Visibility = Visibility.Collapsed;
    }
}

private void CompactOverlayButton_Click(object sender, RoutedEventArgs e)
{
    if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.CompactOverlay)
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.CompactOverlay);
        fullScreenButton.IsChecked = false;
    }
    else
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
    }
}

private void FullScreenButton_Click(object sender, RoutedEventArgs e)
{
    if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.FullScreen)
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.FullScreen);
        compactOverlayButton.IsChecked = false;
    }
    else
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
    }
}
```

## <a name="reuse-xaml-elements"></a>Reutilizar elementos XAML

Un [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) permite tener varios árboles XAML con el mismo subproceso de interfaz de usuario. Sin embargo, un elemento XAML solo se puede Agregar una vez a un árbol XAML. Si quiere trasladar una parte de la interfaz de usuario de una ventana a otra, tiene que administrar su ubicación en el árbol XAML.

En este ejemplo se muestra cómo reutilizar un control [ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker) mientras lo mueve entre la ventana principal y una ventana secundaria.

El selector de colores se declara en el código XAML `MainPage`de, que lo coloca en `MainPage` el árbol XAML.

```xaml
<StackPanel x:Name="colorPickerContainer" Grid.Column="1" Background="WhiteSmoke">
    <Button Click="DetachColorPickerButton_Click" HorizontalAlignment="Right">
        <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE2B4;" />
    </Button>
    <ColorPicker x:Name="colorPicker" Margin="12" Width="288"
                 IsColorChannelTextInputVisible="False"
                 ColorChanged="ColorPicker_ColorChanged"/>
</StackPanel>
```

Cuando el selector de colores se separa para colocarse en un nuevo AppWindow, primero debe quitarlo `MainPage` del árbol XAML si lo quita de su contenedor primario. Aunque no es necesario, en este ejemplo también se oculta el contenedor principal.

```csharp
colorPickerContainer.Children.Remove(colorPicker);
colorPickerContainer.Visibility = Visibility.Collapsed;
```

Después, puede agregarlo al nuevo árbol XAML. En este caso, primero se crea una [cuadrícula](/uwp/api/windows.ui.xaml.controls.grid) que será el contenedor principal de ColorPicker y se agregará ColorPicker como un elemento secundario de la cuadrícula. (Esto le permite quitar más fácilmente el ColorPicker de este árbol XAML). A continuación, establezca la cuadrícula como la raíz del árbol XAML en la nueva ventana.

```csharp
Grid appWindowRootGrid = new Grid();
appWindowRootGrid.Children.Add(colorPicker);

// Create a new window
colorPickerAppWindow = await AppWindow.TryCreateAsync();

// Attach the XAML content to our window
ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);
```

Cuando el [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) está cerrado, se invierte el proceso. En primer lugar, quite [ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker) de la [cuadrícula](/uwp/api/windows.ui.xaml.controls.grid)y, a continuación, agréguelo como elemento secundario de `MainPage` [StackPanel](/uwp/api/windows.ui.xaml.controls.stackpanel) en.

```csharp
// When the window is closed, be sure to release XAML resources
// and the reference to the window.
colorPickerAppWindow.Closed += delegate
{
    appWindowRootGrid.Children.Remove(colorPicker);
    appWindowRootGrid = null;
    colorPickerAppWindow = null;

    colorPickerContainer.Children.Add(colorPicker);
    colorPickerContainer.Visibility = Visibility.Visible;
};
```

```csharp
private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
{
    ColorPickerContainer.Visibility = Visibility.Collapsed;

    // Create the color picker window.
    if (colorPickerAppWindow == null)
    {
        ColorPickerContainer.Children.Remove(colorPicker);

        Grid appWindowRootGrid = new Grid();
        appWindowRootGrid.Children.Add(colorPicker);

        // Create a new window
        colorPickerAppWindow = await AppWindow.TryCreateAsync();
        colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
        colorPickerAppWindow.RequestSize(new Size(300, 428));
        colorPickerAppWindow.Title = "Color picker";

        // Attach the XAML content to our window
        ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);

        // When the window is closed, be sure to release XAML resources
        // and the reference to the window.
        colorPickerAppWindow.Closed += delegate
        {
            appWindowRootGrid.Children.Remove(colorPicker);
            appWindowRootGrid = null;
            colorPickerAppWindow = null;

            ColorPickerContainer.Children.Add(colorPicker);
            ColorPickerContainer.Visibility = Visibility.Visible;
        };
    }
    // Show the window.
    await colorPickerAppWindow.TryShowAsync();
}
```

## <a name="show-a-dialog-box"></a>Mostrar un cuadro de diálogo

De manera predeterminada, los cuadros de diálogo muestran de forma modal con respecto a la raíz [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview). Cuando se usa un [ContentDialog](/uwp/api/windows.ui.xaml.controls.contentdialog) dentro de un [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow), es necesario establecer manualmente el valor de XamlRoot en el cuadro de diálogo en la raíz del host XAML.

Para ello, establezca la propiedad [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) de ContentDialog en la misma [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) que un elemento que ya está en AppWindow. Aquí, este código está dentro del controlador de eventos [click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) de un botón, por lo que puede usar el _remitente_ (el botón en el que hizo clic) para obtener el XamlRoot.

```csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
{
    simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
}
```

Si tiene uno o varios AppWindows abiertos además de la ventana principal (ApplicationView), cada ventana puede intentar abrir un cuadro de diálogo, ya que el cuadro de diálogo modal solo bloqueará la ventana en la que se ha modificado. Sin embargo, solo puede haber un [ContentDialog](/uwp/api/windows.ui.xaml.controls.contentdialog) abierto por subproceso a la vez. Al intentar abrir dos ContentDialogs producirá una excepción, incluso si está intentando abrir en AppWindows independiente.

Para administrar esto, al menos debe abrir el cuadro de diálogo en un `try/catch` bloque para detectar la excepción en caso de que ya haya otro cuadro de diálogo abierto.

```csharp
try
{
    ContentDialogResult result = await simpleDialog.ShowAsync();
}
catch (Exception)
{
    // The dialog didn't open, probably because another dialog is already open.
}
```

Otra manera de administrar los diálogos consiste en realizar un seguimiento del cuadro de diálogo abierto y cerrarlo antes de intentar abrir un cuadro de diálogo nuevo. En este caso, se crea una propiedad `MainPage` estática `CurrentDialog` en llamada para este propósito.

```csharp
public sealed partial class MainPage : Page
{
    // Track the last opened dialog so you can close it if another dialog tries to open.
    public static ContentDialog CurrentDialog { get; set; } = null;

   // ...
}
```

A continuación, compruebe si hay un cuadro de diálogo actualmente abierto y, si lo hay, llame al método [Hide](/uwp/api/windows.ui.xaml.controls.contentdialog.hide) para cerrarlo. Por último, asigne el nuevo cuadro `CurrentDialog`de diálogo a e intente mostrarlo.

```csharp
private async void DialogButton_Click(object sender, RoutedEventArgs e)
{
    ContentDialog simpleDialog = new ContentDialog
    {
        Title = "Content dialog",
        Content = "Dialog box for " + window.Title,
        CloseButtonText = "Ok"
    };

    if (MainPage.CurrentDialog != null)
    {
        MainPage.CurrentDialog.Hide();
    }
    MainPage.CurrentDialog = simpleDialog;

    // Use this code to associate the dialog to the appropriate AppWindow by setting
    // the dialog's XamlRoot to the same XamlRoot as an element that is already
    // present in the AppWindow.
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
    {
        simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
    }

    try
    {
        ContentDialogResult result = await simpleDialog.ShowAsync();
    }
    catch (Exception)
    {
        // The dialog didn't open, probably because another dialog is already open.
    }
}
```

Si no es deseable que un cuadro de diálogo se cierre mediante programación, no lo asigne `CurrentDialog`como. Aquí, `MainPage` se muestra un cuadro de diálogo importante que solo se debe descartar cuando se `Ok`haga clic en el uso. Dado que no se asigna como `CurrentDialog`, no se realiza ningún intento para cerrarlo mediante programación.

```csharp
public sealed partial class MainPage : Page
{
    // Track the last opened dialog so you can close it if another dialog tries to open.
    public static ContentDialog CurrentDialog { get; set; } = null;

    // ...
    private async void DialogButton_Click(object sender, RoutedEventArgs e)
    {
        ContentDialog importantDialog = new ContentDialog
        {
            Title = "Important dialog",
            Content = "This dialog can only be dismissed by clicking Ok.",
            CloseButtonText = "Ok"
        };

        if (MainPage.CurrentDialog != null)
        {
            MainPage.CurrentDialog.Hide();
        }
        // Do not track this dialog as the MainPage.CurrentDialog.
        // It should only be closed by clicking the Ok button.
        MainPage.CurrentDialog = null;

        try
        {
            ContentDialogResult result = await importantDialog.ShowAsync();
        }
        catch (Exception)
        {
            // The dialog didn't open, probably because another dialog is already open.
        }
    }
    // ...
}
```

## <a name="complete-code"></a>Código completo

### <a name="mainpagexaml"></a>MainPage.xaml

```xaml
<Page
    x:Class="HelloAppWindow.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloAppWindow"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <Button x:Name="NewWindowButton" Content="Open new window" 
                    Click="ShowNewWindowButton_Click" Margin="0,12"/>
            <Button Content="Open dialog" Click="DialogButton_Click" 
                    HorizontalAlignment="Stretch"/>
            <Button Content="Close all" Click="CloseAllButton_Click" 
                    Margin="0,12" HorizontalAlignment="Stretch"/>
        </StackPanel>

<StackPanel x:Name="colorPickerContainer" Grid.Column="1" Background="WhiteSmoke">
    <Button Click="DetachColorPickerButton_Click" HorizontalAlignment="Right">
        <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE2B4;" />
    </Button>
            <ColorPicker x:Name="colorPicker" Margin="12" Width="288"
                 IsColorChannelTextInputVisible="False"
                 ColorChanged="ColorPicker_ColorChanged"/>
        </StackPanel>
    </Grid>
</Page>

```

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Windows.Foundation;
using Windows.UI;
using Windows.UI.WindowManagement;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Hosting;
using Windows.UI.Xaml.Media;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=402352&clcid=0x409

namespace HelloAppWindow
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        AppWindow colorPickerAppWindow;

        // Track open app windows in a Dictionary.
        public static Dictionary<UIContext, AppWindow> AppWindows { get; set; }
            = new Dictionary<UIContext, AppWindow>();

        // Track the last opened dialog so you can close it if another dialog tries to open.
        public static ContentDialog CurrentDialog { get; set; } = null;

        public MainPage()
        {
            this.InitializeComponent();
        }

        private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
        {
            // Create a new window.
            AppWindow appWindow = await AppWindow.TryCreateAsync();

            // Create a Frame and navigate to the Page you want to show in the new window.
            Frame appWindowContentFrame = new Frame();
            appWindowContentFrame.Navigate(typeof(AppWindowPage));

            // Get a reference to the page instance and assign the
            // newly created AppWindow to the MyAppWindow property.
            AppWindowPage page = (AppWindowPage)appWindowContentFrame.Content;
            page.MyAppWindow = appWindow;
            page.TextColorBrush = new SolidColorBrush(colorPicker.Color);

            // Attach the XAML content to the window.
            ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);

            // Add the new page to the Dictionary using the UIContext as the Key.
            AppWindows.Add(appWindowContentFrame.UIContext, appWindow);
            appWindow.Title = "App Window " + AppWindows.Count.ToString();

            // When the window is closed, be sure to release XAML resources
            // and the reference to the window.
            appWindow.Closed += delegate
            {
                MainPage.AppWindows.Remove(appWindowContentFrame.UIContext);
                appWindowContentFrame.Content = null;
                appWindow = null;
            };

            // Show the window.
            await appWindow.TryShowAsync();
        }

        private async void DialogButton_Click(object sender, RoutedEventArgs e)
        {
            ContentDialog importantDialog = new ContentDialog
            {
                Title = "Important dialog",
                Content = "This dialog can only be dismissed by clicking Ok.",
                CloseButtonText = "Ok"
            };

            if (MainPage.CurrentDialog != null)
            {
                MainPage.CurrentDialog.Hide();
            }
            // Do not track this dialog as the MainPage.CurrentDialog.
            // It should only be closed by clicking the Ok button.
            MainPage.CurrentDialog = null;

            try
            {
                ContentDialogResult result = await importantDialog.ShowAsync();
            }
            catch (Exception)
            {
                // The dialog didn't open, probably because another dialog is already open.
            }
        }

        private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
        {
            // Create the color picker window.
            if (colorPickerAppWindow == null)
            {
                colorPickerContainer.Children.Remove(colorPicker);
                colorPickerContainer.Visibility = Visibility.Collapsed;

                Grid appWindowRootGrid = new Grid();
                appWindowRootGrid.Children.Add(colorPicker);

                // Create a new window
                colorPickerAppWindow = await AppWindow.TryCreateAsync();
                colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
                colorPickerAppWindow.RequestSize(new Size(300, 428));
                colorPickerAppWindow.Title = "Color picker";

                // Attach the XAML content to our window
                ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);

                // Make sure to release the reference to this window, 
                // and release XAML resources, when it's closed
                colorPickerAppWindow.Closed += delegate
                {
                    appWindowRootGrid.Children.Remove(colorPicker);
                    appWindowRootGrid = null;
                    colorPickerAppWindow = null;

                    colorPickerContainer.Children.Add(colorPicker);
                    colorPickerContainer.Visibility = Visibility.Visible;
                };
            }
            // Show the window.
            await colorPickerAppWindow.TryShowAsync();
        }

        private void ColorPicker_ColorChanged(ColorPicker sender, ColorChangedEventArgs args)
        {
            NewWindowButton.Background = new SolidColorBrush(args.NewColor);
        }

        private async void CloseAllButton_Click(object sender, RoutedEventArgs e)
        {
            while (AppWindows.Count > 0)
            {
                await AppWindows.Values.First().CloseAsync();
            }
        }
    }
}

```

### <a name="appwindowpagexaml"></a>AppWindowPage. Xaml

```xaml
<Page
    x:Class="HelloAppWindow.AppWindowPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloAppWindow"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid>
        <TextBlock x:Name="TitleTextBlock" Text="Hello AppWindow!" FontSize="24" HorizontalAlignment="Center" Margin="24"/>

        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <Button Content="Open dialog" Click="DialogButton_Click"
                    Width="200" Margin="0,4"/>
            <Button Content="Move window" Click="MoveWindowButton_Click"
                    Width="200" Margin="0,4"/>
            <ToggleButton Content="Compact Overlay" x:Name="compactOverlayButton" Click="CompactOverlayButton_Click"
                          Width="200" Margin="0,4"/>
            <ToggleButton Content="Full Screen" x:Name="fullScreenButton" Click="FullScreenButton_Click"
                          Width="200" Margin="0,4"/>
            <Grid>
                <TextBlock Text="Size:"/>
                <TextBlock x:Name="SizeText" HorizontalAlignment="Right"/>
            </Grid>
            <Grid>
                <TextBlock Text="Presentation:"/>
                <TextBlock x:Name="ConfigText" HorizontalAlignment="Right"/>
            </Grid>
        </StackPanel>
    </Grid>
</Page>
```

### <a name="appwindowpagexamlcs"></a>AppWindowPage.xaml.cs

```csharp
using System;
using Windows.Foundation;
using Windows.Foundation.Metadata;
using Windows.UI;
using Windows.UI.WindowManagement;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=234238

namespace HelloAppWindow
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class AppWindowPage : Page
    {
        AppWindow window;

        public AppWindow MyAppWindow { get; set; }

        public SolidColorBrush TextColorBrush { get; set; } = new SolidColorBrush(Colors.Black);

        public AppWindowPage()
        {
            this.InitializeComponent();

            Loaded += AppWindowPage_Loaded;
        }

        private void AppWindowPage_Loaded(object sender, RoutedEventArgs e)
        {
            // Get the reference to this AppWindow that was stored when it was created.
            window = MainPage.AppWindows[this.UIContext];

            // Set up event handlers for the window.
            window.Changed += Window_Changed;

            TitleTextBlock.Foreground = TextColorBrush;
        }

        private async void DialogButton_Click(object sender, RoutedEventArgs e)
        {
            ContentDialog simpleDialog = new ContentDialog
            {
                Title = "Content dialog",
                Content = "Dialog box for " + window.Title,
                CloseButtonText = "Ok"
            };

            if (MainPage.CurrentDialog != null)
            {
                MainPage.CurrentDialog.Hide();
            }
            MainPage.CurrentDialog = simpleDialog;

            // Use this code to associate the dialog to the appropriate AppWindow by setting
            // the dialog's XamlRoot to the same XamlRoot as an element that is already 
            // present in the AppWindow.
            if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
            {
                simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
            }

            try
            {
                ContentDialogResult result = await simpleDialog.ShowAsync();
            }
            catch (Exception)
            {
                // The dialog didn't open, probably because another dialog is already open.
            }
        }

        private void Window_Changed(AppWindow sender, AppWindowChangedEventArgs args)
        {
            if (args.DidAvailableWindowPresentationsChange)
            {
                EnablePresentationButtons(sender);
            }

            if (args.DidWindowPresentationChange)
            {
                ConfigText.Text = window.Presenter.GetConfiguration().Kind.ToString();
            }

            if (args.DidSizeChange)
            {
                SizeText.Text = window.GetPlacement().Size.ToString();
            }
        }

        private void EnablePresentationButtons(AppWindow window)
        {
            // Check whether the current AppWindowPresenter supports CompactOverlay.
            if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.CompactOverlay))
            {
                // Show the CompactOverlay button...
                compactOverlayButton.Visibility = Visibility.Visible;
            }
            else
            {
                // Hide the CompactOverlay button...
                compactOverlayButton.Visibility = Visibility.Collapsed;
            }

            // Check whether the current AppWindowPresenter supports FullScreen?
            if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.FullScreen))
            {
                // Show the FullScreen button...
                fullScreenButton.Visibility = Visibility.Visible;
            }
            else
            {
                // Hide the FullScreen button...
                fullScreenButton.Visibility = Visibility.Collapsed;
            }
        }

        private void CompactOverlayButton_Click(object sender, RoutedEventArgs e)
        {
            if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.CompactOverlay)
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.CompactOverlay);
                fullScreenButton.IsChecked = false;
            }
            else
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
            }
        }

        private void FullScreenButton_Click(object sender, RoutedEventArgs e)
        {
            if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.FullScreen)
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.FullScreen);
                compactOverlayButton.IsChecked = false;
            }
            else
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
            }
        }

        private void MoveWindowButton_Click(object sender, RoutedEventArgs e)
        {
            DisplayRegion displayRegion = window.GetPlacement().DisplayRegion;
            double displayRegionWidth = displayRegion.WorkAreaSize.Width;
            double windowWidth = window.GetPlacement().Size.Width;
            int horizontalOffset = (int)(displayRegionWidth - windowWidth);
            window.RequestMoveRelativeToDisplayRegion(displayRegion, new Point(horizontalOffset, 0));
        }
    }
}
```

## <a name="related-topics"></a>Temas relacionados

- [Mostrar varias vistas](show-multiple-views.md)
- [Mostrar varias vistas con ApplicationView](application-view.md)