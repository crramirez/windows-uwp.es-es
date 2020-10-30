---
description: Simule y automatice la entrada desde dispositivos como el teclado, el mouse, el toque, el lápiz y el controlador para juegos en las aplicaciones de Windows.
title: Simular la entrada del usuario a través de la inserción de entrada
label: Input injection
template: detail.hbs
keywords: dispositivo, digitalizador, entrada, interacción, inyección
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0cd1a56ca46c3e9ea401794ff5b9964545ce0c5d
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030128"
---
# <a name="simulate-user-input-through-input-injection"></a>Simular la entrada del usuario a través de la inserción de entrada

Simule y automatice los datos proporcionados por el usuario desde dispositivos como el teclado, el mouse, el toque, el lápiz y el controlador para juegos en las aplicaciones Windows.

> **API importantes** : [ **Windows. UI. Input. preview. inyecciones**](/uwp/api/windows.ui.input.preview.injection)

## <a name="overview"></a>Introducción

La inserción de entradas permite a la aplicación Windows simular la entrada desde una variedad de dispositivos de entrada y dirigir esa entrada en cualquier parte, incluido fuera del área cliente de la aplicación (incluso en las aplicaciones que se ejecutan con privilegios de administrador, como el editor del registro).

La inserción de entrada es útil para las aplicaciones y herramientas de Windows que necesitan proporcionar funcionalidad que incluye características de accesibilidad, pruebas (ad hoc, automatizadas) y acceso remoto y soporte técnico.

## <a name="setup"></a>Configuración

Para usar las API de inserción de entrada en la aplicación de Windows, deberá agregar lo siguiente al manifiesto de la aplicación:

1. Haga clic con el botón derecho en el archivo **Package. appxmanifest** y seleccione **Ver código** .
1. Inserte lo siguiente en el `Package` nodo:
    - `xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"`
    - `IgnorableNamespaces="rescap"`
1. Inserte lo siguiente en el `Capabilities` nodo:
    - `<rescap:Capability Name="inputInjectionBrokered" />`

## <a name="duplicate-user-input"></a>Entrada de usuario duplicada

| ![Ejemplo de inyección de entrada táctil](images/injection/touch-input-injection.gif) | 
|:--:|
| *Ejemplo de inyección de entrada táctil* |

En este ejemplo, se muestra cómo usar las API de inserción de entrada ([Windows. UI. Input. preview. inyecciones](/uwp/api/windows.ui.input.preview.injection)) para escuchar eventos de entrada del mouse en una región de una aplicación y simular los eventos de entrada táctil correspondientes en otra región.

**Descargar este ejemplo del [ejemplo de inyección de entrada (mouse en Touch)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-input-injection-mouse-to-touch.zip)**

1. En primer lugar, se configura la interfaz de usuario (MainPage. xaml).

    Tenemos dos áreas de cuadrícula (una para la entrada del mouse y otra para la entrada táctil insertada), cada una con cuatro botones.
      > [!NOTE] 
      > Se debe asignar un valor (, en este caso) al fondo de cuadrícula; de `Transparent` lo contrario, no se detectan eventos de puntero.

    Cuando se detectan clics del mouse en el área de entrada, se inserta un evento Touch correspondiente en el área de inyección de entrada. Los clics de botón de entrada de inserción se muestran en el área de título.

    ```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel Grid.Row="0"
                    Margin="10">
            <TextBlock Style="{ThemeResource TitleTextBlockStyle}" 
                       Name="titleText"
                       Text="Touch input injection"
                       HorizontalTextAlignment="Center" />
            <TextBlock Style="{ThemeResource BodyTextBlockStyle}"
                       Name="statusText"
                       HorizontalTextAlignment="Center" />
        </StackPanel>
        <Grid HorizontalAlignment="Center"
                        Grid.Row="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Column="0" 
                       Grid.Row="0" 
                       Style="{ThemeResource CaptionTextBlockStyle}"
                       Text="User mouse input area"/>
            <!-- Background must be set to something, otherwise pointer events are not detected. -->
            <Grid Name="ContainerInput" 
                  Grid.Column="0" 
                  Grid.Row="1"
                  HorizontalAlignment="Stretch" 
                  Background="Transparent" 
                  BorderBrush="Green" 
                  BorderThickness="2" 
                  MinHeight="100" MinWidth="300" 
                  Margin="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Button Name="B1" 
                        Grid.Column="0" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50"
                        Content="B1" />
                <Button Name="B2" 
                        Grid.Column="1" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50"
                        Content="B2" />
                <Button Name="B3" 
                        Grid.Column="2" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50"
                        Content="B3" />
                <Button Name="B4" 
                        Grid.Column="3" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50"
                        Content="B4" />
            </Grid>
            <TextBlock Grid.Column="1" 
                       Grid.Row="0"                         
                       Style="{ThemeResource CaptionTextBlockStyle}"
                       Text="Injected touch input area"/>
            <Grid Name="ContainerInject"
                  Grid.Column="1"  
                  Grid.Row="1"
                  HorizontalAlignment="Stretch"
                  BorderBrush="Red" 
                  BorderThickness="2" 
                  MinHeight="100" MinWidth="300" 
                  Margin="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Button Name="B1i" Click="Button_Click_Injected"
                        Content="B1i"
                        Grid.Column="0" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50" />
                <Button Name="B2i" Click="Button_Click_Injected"
                        Content="B2i"
                        Grid.Column="1" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50" />
                <Button Name="B3i" Click="Button_Click_Injected"
                        Content="B3i"
                        Grid.Column="2" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50" />
                <Button Name="B4i" Click="Button_Click_Injected"
                        Content="B4i"
                        Grid.Column="3" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50" />
            </Grid>
        </Grid>
    </Grid>
    ```

2. A continuación, inicializamos nuestra aplicación.
    
    En este fragmento de código, se declaran los objetos globales y se declaran los agentes de escucha para los eventos de puntero ([AddHandler](/uwp/api/windows.ui.xaml.uielement.addhandler)) dentro del área de entrada del mouse que se puede marcar como controlado en los eventos de clic de botón.

    El objeto [InputInjector](/uwp/api/windows.ui.input.preview.injection.inputinjector) representa el dispositivo de entrada virtual para enviar los datos de entrada.

    En el `ContainerInput_PointerPressed` controlador se llama a la función de inyección táctil.

    En el `ContainerInput_PointerReleased` controlador se llama a UninitializeTouchInjection para cerrar el objeto [InputInjector](/uwp/api/windows.ui.input.preview.injection.inputinjector) .

    ```csharp
    public sealed partial class MainPage : Page
    {
        /// <summary>
        /// The virtual input device.
        /// </summary>
        InputInjector _inputInjector;

        /// <summary>
        /// Initialize the app, set the window size, 
        /// and add pointer input handlers for the container.
        /// </summary>
        public MainPage()
        {
            this.InitializeComponent();

            ApplicationView.PreferredLaunchViewSize =
                new Size(600, 200);
            ApplicationView.PreferredLaunchWindowingMode =
                ApplicationViewWindowingMode.PreferredLaunchViewSize;

            // Button handles PointerPressed/PointerReleased in 
            // the Tapped routed event, but we need the container Grid 
            // to handle them also. Add a handler for both 
            // PointerPressedEvent and PointerReleasedEvent on the input Grid 
            // and set handledEventsToo to true.
            ContainerInput.AddHandler(PointerPressedEvent,
                new PointerEventHandler(ContainerInput_PointerPressed), true);
            ContainerInput.AddHandler(PointerReleasedEvent,
                new PointerEventHandler(ContainerInput_PointerReleased), true);
        }

        /// <summary>
        /// PointerReleased handler for all pointer conclusion events.
        /// PointerPressed and PointerReleased events do not always occur 
        /// in pairs, so your app should listen for and handle any event that 
        /// might conclude a pointer down (such as PointerExited, PointerCanceled, 
        /// and PointerCaptureLost).  
        /// </summary>
        /// <param name="sender">Source of the click event</param>
        /// <param name="e">Event args for the button click routed event</param>
        private void ContainerInput_PointerReleased(
            object sender, PointerRoutedEventArgs e)
        {
            // Prevent most handlers along the event route from handling event again.
            e.Handled = true;

            // Shut down the virtual input device.
            _inputInjector.UninitializeTouchInjection();
        }

        /// <summary>
        /// PointerPressed handler.
        /// PointerPressed and PointerReleased events do not always occur 
        /// in pairs. Your app should listen for and handle any event that 
        /// might conclude a pointer down (such as PointerExited, 
        /// PointerCanceled, and PointerCaptureLost).  
        /// </summary>
        /// <param name="sender">Source of the click event</param>
        /// <param name="e">Event args for the button click routed event</param>
        private void ContainerInput_PointerPressed(
            object sender, PointerRoutedEventArgs e)
        {
            // Prevent most handlers along the event route from 
            // handling the same event again.
            e.Handled = true;

            InjectTouchForMouse(e.GetCurrentPoint(ContainerInput));

        }
        ...
    }
    ```
3. Esta es la función de inserción de entrada táctil.

    En primer lugar, llamamos a [TryCreate](/uwp/api/windows.ui.input.preview.injection.inputinjector.trycreate) para crear una instancia del objeto [InputInjector](/uwp/api/windows.ui.input.preview.injection.inputinjector) .

    A continuación, llamamos a [InitializeTouchInjection](/uwp/api/windows.ui.input.preview.injection.inputinjector.initializetouchinjection) con un [InjectedInputVisualizationMode](/uwp/api/windows.ui.input.preview.injection.injectedinputvisualizationmode) de `Default` .

    Después de calcular el punto de inserción, llamamos a [InjectedInputTouchInfo](/uwp/api/windows.ui.input.preview.injection.injectedinputtouchinfo) para inicializar la lista de puntos táctiles que se van a insertar (en este ejemplo, se crea un punto de toque correspondiente al puntero de entrada del mouse).

    Por último, llamamos a [InjectTouchInput](/uwp/api/windows.ui.input.preview.injection.inputinjector.injecttouchinput) dos veces, la primera para un puntero hacia abajo y la segunda para un puntero hacia arriba.

    ```csharp
    /// <summary>
    /// Inject touch input on injection target corresponding 
    /// to mouse click on input target.
    /// </summary>
    /// <param name="pointerPoint">The mouse click pointer.</param>
    private void InjectTouchForMouse(PointerPoint pointerPoint)
    {
        // Create the touch injection object.
        _inputInjector = InputInjector.TryCreate();

        if (_inputInjector != null)
        {
            _inputInjector.InitializeTouchInjection(
                InjectedInputVisualizationMode.Default);

            // Create a unique pointer ID for the injected touch pointer.
            // Multiple input pointers would require more robust handling.
            uint pointerId = pointerPoint.PointerId + 1;

            // Get the bounding rectangle of the app window.
            Rect appBounds =
                Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().VisibleBounds;

            // Get the top left screen coordinates of the app window rect.
            Point appBoundsTopLeft = new Point(appBounds.Left, appBounds.Top);

            // Get a reference to the input injection area.
            GeneralTransform injectArea =
                ContainerInject.TransformToVisual(Window.Current.Content);

            // Get the top left screen coordinates of the input injection area.
            Point injectAreaTopLeft = injectArea.TransformPoint(new Point(0, 0));

            // Get the screen coordinates (relative to the input area) 
            // of the input pointer.
            int pointerPointX = (int)pointerPoint.Position.X;
            int pointerPointY = (int)pointerPoint.Position.Y;

            // Create the point for input injection and calculate its screen location.
            Point injectionPoint =
                new Point(
                    appBoundsTopLeft.X + injectAreaTopLeft.X + pointerPointX,
                    appBoundsTopLeft.Y + injectAreaTopLeft.Y + pointerPointY);

            // Create a touch data point for pointer down.
            // Each element in the touch data list represents a single touch contact. 
            // For this example, we're mirroring a single mouse pointer.
            List<InjectedInputTouchInfo> touchData =
                new List<InjectedInputTouchInfo>
                {
                    new InjectedInputTouchInfo
                    {
                        Contact = new InjectedInputRectangle
                        {
                            Left = 30, Top = 30, Bottom = 30, Right = 30
                        },
                        PointerInfo = new InjectedInputPointerInfo
                        {
                            PointerId = pointerId,
                            PointerOptions =
                            InjectedInputPointerOptions.PointerDown |
                            InjectedInputPointerOptions.InContact |
                            InjectedInputPointerOptions.New,
                            TimeOffsetInMilliseconds = 0,
                            PixelLocation = new InjectedInputPoint
                            {
                                PositionX = (int)injectionPoint.X ,
                                PositionY = (int)injectionPoint.Y
                            }
                    },
                    Pressure = 1.0,
                    TouchParameters =
                        InjectedInputTouchParameters.Pressure |
                        InjectedInputTouchParameters.Contact
                }
            };

            // Inject the touch input. 
            _inputInjector.InjectTouchInput(touchData);

            // Create a touch data point for pointer up.
            touchData = new List<InjectedInputTouchInfo>
            {
                new InjectedInputTouchInfo
                {
                    PointerInfo = new InjectedInputPointerInfo
                    {
                        PointerId = pointerId,
                        PointerOptions = InjectedInputPointerOptions.PointerUp
                    }
                }
            };

            // Inject the touch input. 
            _inputInjector.InjectTouchInput(touchData);
        }
    }
    ```

4. Por último, se controla cualquier evento enrutado de [clic](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase) de botón en el área de inyección de entrada y se actualiza la interfaz de usuario con el nombre del botón en el que se hizo clic.

## <a name="see-also"></a>Consulte también

### <a name="topic-samples"></a>Ejemplos del tema

- [Ejemplo de inyección de entrada (mouse en Touch)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-input-injection-mouse-to-touch.zip)
