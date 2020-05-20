---
title: Interacciones de miras
Description: Obtenga información sobre cómo diseñar y optimizar las aplicaciones de Windows para proporcionar la mejor experiencia posible a los usuarios que confían en la entrada de la mirada del ojo y del cabeza.
label: Gaze interactions
template: detail.hbs
keywords: mira fijamente, seguimiento ocular, seguimiento del cabezal, punto de miración, entrada, interacción del usuario, accesibilidad, facilidad de uso
ms.date: 05/01/2018
ms.topic: article
pm-contact: Jake Cohen
dev-contact: Austin Hodges
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4cfd84d54ecd1425b3b7e66c54c96fbd78c2dd46
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970130"
---
# <a name="gaze-interactions-and-eye-tracking-in-windows-apps"></a>Interacciones de mirada y seguimiento ocular en aplicaciones de Windows

![Héroe de seguimiento de ojos](images/gaze/eyecontrolbanner1.png)

Proporcione soporte para realizar un seguimiento de la mirada, la atención y la presencia de un usuario en función de la ubicación y el movimiento de sus ojos.

> [!NOTE]
> Para ver la entrada de mira en [Windows Mixed Reality](https://docs.microsoft.com/windows/mixed-reality/), consulte [mira fijamente](https://docs.microsoft.com/windows/mixed-reality/gaze).

**API importantes**: [Windows. Devices. Input. preview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview), [GazeDevicePreview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicepreview), [GazePointPreview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazepointpreview), [GazeInputSourcePreview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview)

## <a name="overview"></a>Información general

La entrada de mirada es una manera eficaz de interactuar y usar aplicaciones Windows que es especialmente útil como tecnología de asistencia para los usuarios con enfermedades neuro-muscular (por ejemplo, ALS) y otras discapacidades que implican funciones no emparejadas de músculo o Nerve.

Además, la entrada de mirada ofrece oportunidades igualmente atractivas para juegos (incluidos la adquisición y el seguimiento de destino) y las aplicaciones de productividad tradicionales, quioscos y otros escenarios interactivos en los que los dispositivos de entrada tradicionales (teclado, Mouse, Touch) no están disponibles, o en los que pueden resultar útiles o útiles para liberar las manos del usuario para otras tareas (por ejemplo,

> [!NOTE]
> La compatibilidad con el hardware de seguimiento de ojos se presentó en **Windows 10 Fall Creators Update** junto con el [control ocular](https://support.microsoft.com/help/4043921/windows-10-get-started-eye-control), una característica integrada que le permite usar los ojos para controlar el puntero en pantalla, escribir con el teclado en pantalla y comunicarse con personas que usan texto a voz. Un conjunto de API de Windows Runtime ([Windows. Devices. Input. preview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview)) para compilar aplicaciones que pueden interactuar con el hardware de seguimiento de ojos está disponible con la **actualización 2018 de abril de Windows 10 (versión 1803, compilación 17134)** y versiones más recientes.

## <a name="privacy"></a>Privacidad

Debido a los datos personales potencialmente confidenciales recopilados por los dispositivos de seguimiento ocular, es necesario declarar la `gazeInput` funcionalidad en el manifiesto de la aplicación de la aplicación (consulte la sección **configuración** siguiente). Cuando se declara, Windows solicita automáticamente a los usuarios un cuadro de diálogo de consentimiento (cuando la aplicación se ejecuta por primera vez), donde el usuario debe conceder permiso para que la aplicación se comunique con el dispositivo de seguimiento ocular y obtener acceso a estos datos.

Además, si la aplicación recopila, almacena o transfiere datos de seguimiento ocular, debes describirlo en la declaración de privacidad de la aplicación y seguir todos los demás requisitos para obtener **información personal** en el [contrato para desarrolladores de aplicaciones](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) y las directivas de [Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies).

## <a name="setup"></a>Configuración

Para usar las API de entrada de fijamente en la aplicación de Windows, necesitará: 

- Especifique la `gazeInput` funcionalidad en el manifiesto de la aplicación.

    Abra el archivo **Package. appxmanifest** con el diseñador de manifiestos de Visual Studio o agregue la funcionalidad manualmente; para ello, seleccione **Ver código**e inserte lo siguiente `DeviceCapability` en el `Capabilities` nodo:

    ```xaml
    <Capabilities>
       <DeviceCapability Name="gazeInput" />
    </Capabilities>
    ```

- Un dispositivo de seguimiento ocular compatible con Windows conectado al sistema (ya sea integrado o periférico) y activado.

    Consulte Introducción [al control ocular en Windows 10](https://support.microsoft.com/help/4043921/windows-10-get-started-eye-control#supported-devices ) para obtener una lista de los dispositivos de seguimiento ocular admitidos.

## <a name="basic-eye-tracking"></a>Seguimiento de ojos básicos

En este ejemplo, se muestra cómo realizar un seguimiento de la mirada del usuario en una aplicación de Windows y cómo usar una función de control de tiempo con la prueba de posicionamiento básica para indicar hasta qué punto pueden mantener su enfoque de miración en un elemento específico.

Se utiliza una elipse pequeña para mostrar dónde se encuentra el punto de mira en la ventanilla de la aplicación, mientras que un [RadialProgressBar](https://docs.microsoft.com/windows/communitytoolkit/controls/radialprogressbar) del kit de herramientas de la [comunidad de Windows](https://docs.microsoft.com/windows/communitytoolkit/) se coloca aleatoriamente en el lienzo. Cuando se detecta el foco de miración en la barra de progreso, se inicia un temporizador y la barra de progreso se reubica aleatoriamente en el lienzo cuando la barra de progreso alcanza el 100%.

![Ejemplo de Tracking with Timer](images/gaze/gaze-input-timed2.gif)

*Ejemplo de Tracking with Timer*

**Descargar este ejemplo de [ejemplo de entrada de mirada (Basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-gazeinput-basic.zip)**

1. En primer lugar, se configura la interfaz de usuario (MainPage. xaml).

    ```xaml
    <Page
        x:Class="gazeinput.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:gazeinput"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:controls="using:Microsoft.Toolkit.Uwp.UI.Controls"    
        mc:Ignorable="d">
    
        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <Grid x:Name="containerGrid">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="*"/>
                </Grid.RowDefinitions>
                <StackPanel x:Name="HeaderPanel" 
                        Orientation="Horizontal" 
                        Grid.Row="0">
                    <StackPanel.Transitions>
                        <TransitionCollection>
                            <AddDeleteThemeTransition/>
                        </TransitionCollection>
                    </StackPanel.Transitions>
                    <TextBlock x:Name="Header" 
                           Text="Gaze tracking sample" 
                           Style="{ThemeResource HeaderTextBlockStyle}" 
                           Margin="10,0,0,0" />
                    <TextBlock x:Name="TrackerCounterLabel"
                           VerticalAlignment="Center"                 
                           Style="{ThemeResource BodyTextBlockStyle}"
                           Text="Number of trackers: " 
                           Margin="50,0,0,0"/>
                    <TextBlock x:Name="TrackerCounter"
                           VerticalAlignment="Center"                 
                           Style="{ThemeResource BodyTextBlockStyle}"
                           Text="0" 
                           Margin="10,0,0,0"/>
                    <TextBlock x:Name="TrackerStateLabel"
                           VerticalAlignment="Center"                 
                           Style="{ThemeResource BodyTextBlockStyle}"
                           Text="State: " 
                           Margin="50,0,0,0"/>
                    <TextBlock x:Name="TrackerState"
                           VerticalAlignment="Center"                 
                           Style="{ThemeResource BodyTextBlockStyle}"
                           Text="n/a" 
                           Margin="10,0,0,0"/>
                </StackPanel>
                <Canvas x:Name="gazePositionCanvas" Grid.Row="1">
                    <controls:RadialProgressBar
                        x:Name="GazeRadialProgressBar" 
                        Value="0"
                        Foreground="Blue" 
                        Background="White"
                        Thickness="4"
                        Minimum="0"
                        Maximum="100"
                        Width="100"
                        Height="100"
                        Outline="Gray"
                        Visibility="Collapsed"/>
                    <Ellipse 
                        x:Name="eyeGazePositionEllipse"
                        Width="20" Height="20"
                        Fill="Blue" 
                        Opacity="0.5" 
                        Visibility="Collapsed">
                    </Ellipse>
                </Canvas>
            </Grid>
        </Grid>
    </Page>
    ```

2. A continuación, inicializamos nuestra aplicación.

    En este fragmento de código, se declaran los objetos globales y se invalida el evento de página [OnNavigatedTo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) para iniciar el [monitor de Device Watcher](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview) y el evento de página [OnNavigatedFrom](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) para detener el monitor de [dispositivo de mirada](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview).

    ```csharp
    using System;
    using Windows.Devices.Input.Preview;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml;
    using Windows.Foundation;
    using System.Collections.Generic;
    using Windows.UI.Xaml.Media;
    using Windows.UI.Xaml.Navigation;
    
    namespace gazeinput
    {
        public sealed partial class MainPage : Page
        {
            /// <summary>
            /// Reference to the user's eyes and head as detected
            /// by the eye-tracking device.
            /// </summary>
            private GazeInputSourcePreview gazeInputSource;
    
            /// <summary>
            /// Dynamic store of eye-tracking devices.
            /// </summary>
            /// <remarks>
            /// Receives event notifications when a device is added, removed, 
            /// or updated after the initial enumeration.
            /// </remarks>
            private GazeDeviceWatcherPreview gazeDeviceWatcher;
    
            /// <summary>
            /// Eye-tracking device counter.
            /// </summary>
            private int deviceCounter = 0;
    
            /// <summary>
            /// Timer for gaze focus on RadialProgressBar.
            /// </summary>
            DispatcherTimer timerGaze = new DispatcherTimer();
    
            /// <summary>
            /// Tracker used to prevent gaze timer restarts.
            /// </summary>
            bool timerStarted = false;
    
            /// <summary>
            /// Initialize the app.
            /// </summary>
            public MainPage()
            {
                InitializeComponent();
            }

            /// <summary>
            /// Override of OnNavigatedTo page event starts GazeDeviceWatcher.
            /// </summary>
            /// <param name="e">Event args for the NavigatedTo event</param>
            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
                // Start listening for device events on navigation to eye-tracking page.
                StartGazeDeviceWatcher();
            }
    
            /// <summary>
            /// Override of OnNavigatedFrom page event stops GazeDeviceWatcher.
            /// </summary>
            /// <param name="e">Event args for the NavigatedFrom event</param>
            protected override void OnNavigatedFrom(NavigationEventArgs e)
            {
                // Stop listening for device events on navigation from eye-tracking page.
                StopGazeDeviceWatcher();
            }
        }
    }
    ```

3. A continuación, agregaremos nuestros métodos de monitor de dispositivo de mira. 
    
    En `StartGazeDeviceWatcher` , se llama a [CreateWatcher](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.createwatcher) y se declaran los agentes de escucha de eventos de dispositivo de seguimiento ocular ([DeviceAdded](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.added), [DeviceUpdated](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.updated)y [DeviceRemoved](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.removed)).

    En `DeviceAdded` , comprobamos el estado del dispositivo de seguimiento ocular. Si se trata de un dispositivo viable, se incrementa el número de dispositivos y se habilita el seguimiento de fijamente. Consulte el paso siguiente para obtener más información.

    En `DeviceUpdated` , también se habilita el seguimiento de miraciones, ya que este evento se desencadena si se recalibra un dispositivo.

    En `DeviceRemoved` , se reduce el contador de dispositivos y se quitan los controladores de eventos del dispositivo.

    En `StopGazeDeviceWatcher` , cerramos el monitor de dispositivo de mira. 

```csharp
    /// <summary>
    /// Start gaze watcher and declare watcher event handlers.
    /// </summary>
    private void StartGazeDeviceWatcher()
    {
        if (gazeDeviceWatcher == null)
        {
            gazeDeviceWatcher = GazeInputSourcePreview.CreateWatcher();
            gazeDeviceWatcher.Added += this.DeviceAdded;
            gazeDeviceWatcher.Updated += this.DeviceUpdated;
            gazeDeviceWatcher.Removed += this.DeviceRemoved;
            gazeDeviceWatcher.Start();
        }
    }

    /// <summary>
    /// Shut down gaze watcher and stop listening for events.
    /// </summary>
    private void StopGazeDeviceWatcher()
    {
        if (gazeDeviceWatcher != null)
        {
            gazeDeviceWatcher.Stop();
            gazeDeviceWatcher.Added -= this.DeviceAdded;
            gazeDeviceWatcher.Updated -= this.DeviceUpdated;
            gazeDeviceWatcher.Removed -= this.DeviceRemoved;
            gazeDeviceWatcher = null;
        }
    }

    /// <summary>
    /// Eye-tracking device connected (added, or available when watcher is initialized).
    /// </summary>
    /// <param name="sender">Source of the device added event</param>
    /// <param name="e">Event args for the device added event</param>
    private void DeviceAdded(GazeDeviceWatcherPreview source, 
        GazeDeviceWatcherAddedPreviewEventArgs args)
    {
        if (IsSupportedDevice(args.Device))
        {
            deviceCounter++;
            TrackerCounter.Text = deviceCounter.ToString();
        }
        // Set up gaze tracking.
        TryEnableGazeTrackingAsync(args.Device);
    }

    /// <summary>
    /// Initial device state might be uncalibrated, 
    /// but device was subsequently calibrated.
    /// </summary>
    /// <param name="sender">Source of the device updated event</param>
    /// <param name="e">Event args for the device updated event</param>
    private void DeviceUpdated(GazeDeviceWatcherPreview source,
        GazeDeviceWatcherUpdatedPreviewEventArgs args)
    {
        // Set up gaze tracking.
        TryEnableGazeTrackingAsync(args.Device);
    }

    /// <summary>
    /// Handles disconnection of eye-tracking devices.
    /// </summary>
    /// <param name="sender">Source of the device removed event</param>
    /// <param name="e">Event args for the device removed event</param>
    private void DeviceRemoved(GazeDeviceWatcherPreview source,
        GazeDeviceWatcherRemovedPreviewEventArgs args)
    {
        // Decrement gaze device counter and remove event handlers.
        if (IsSupportedDevice(args.Device))
        {
            deviceCounter--;
            TrackerCounter.Text = deviceCounter.ToString();

            if (deviceCounter == 0)
            {
                gazeInputSource.GazeEntered -= this.GazeEntered;
                gazeInputSource.GazeMoved -= this.GazeMoved;
                gazeInputSource.GazeExited -= this.GazeExited;
            }
        }
    }
```

4. Aquí, se comprueba si el dispositivo es viable en `IsSupportedDevice` y, en caso afirmativo, se intenta habilitar el seguimiento de miraciones en `TryEnableGazeTrackingAsync` .

    En `TryEnableGazeTrackingAsync` , se declaran los controladores de eventos de mirada y se llama a [GazeInputSourcePreview. GetForCurrentView ()](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.getforcurrentview) para obtener una referencia al origen de entrada (se debe llamar a este método en el subproceso de interfaz de usuario, vea [mantener la capacidad de respuesta del subproceso de interfaz de usuario](https://docs.microsoft.com/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive)).

    > [!NOTE]
    > Solo debe llamar a [GazeInputSourcePreview. GetForCurrentView ()](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.getforcurrentview) cuando un dispositivo de seguimiento ocular compatible esté conectado y requerido por la aplicación. De lo contrario, el cuadro de diálogo de consentimiento no es necesario.

```csharp
    /// <summary>
    /// Initialize gaze tracking.
    /// </summary>
    /// <param name="gazeDevice"></param>
    private async void TryEnableGazeTrackingAsync(GazeDevicePreview gazeDevice)
    {
        // If eye-tracking device is ready, declare event handlers and start tracking.
        if (IsSupportedDevice(gazeDevice))
        {
            timerGaze.Interval = new TimeSpan(0, 0, 0, 0, 20);
            timerGaze.Tick += TimerGaze_Tick;

            SetGazeTargetLocation();

            // This must be called from the UI thread.
            gazeInputSource = GazeInputSourcePreview.GetForCurrentView();

            gazeInputSource.GazeEntered += GazeEntered;
            gazeInputSource.GazeMoved += GazeMoved;
            gazeInputSource.GazeExited += GazeExited;
        }
        // Notify if device calibration required.
        else if (gazeDevice.ConfigurationState ==
                    GazeDeviceConfigurationStatePreview.UserCalibrationNeeded ||
                    gazeDevice.ConfigurationState ==
                    GazeDeviceConfigurationStatePreview.ScreenSetupNeeded)
        {
            // Device isn't calibrated, so invoke the calibration handler.
            System.Diagnostics.Debug.WriteLine(
                "Your device needs to calibrate. Please wait for it to finish.");
            await gazeDevice.RequestCalibrationAsync();
        }
        // Notify if device calibration underway.
        else if (gazeDevice.ConfigurationState == 
            GazeDeviceConfigurationStatePreview.Configuring)
        {
            // Device is currently undergoing calibration.  
            // A device update is sent when calibration complete.
            System.Diagnostics.Debug.WriteLine(
                "Your device is being configured. Please wait for it to finish"); 
        }
        // Device is not viable.
        else if (gazeDevice.ConfigurationState == GazeDeviceConfigurationStatePreview.Unknown)
        {
            // Notify if device is in unknown state.  
            // Reconfigure/recalbirate the device.  
            System.Diagnostics.Debug.WriteLine(
                "Your device is not ready. Please set up your device or reconfigure it."); 
        }
    }

    /// <summary>
    /// Check if eye-tracking device is viable.
    /// </summary>
    /// <param name="gazeDevice">Reference to eye-tracking device.</param>
    /// <returns>True, if device is viable; otherwise, false.</returns>
    private bool IsSupportedDevice(GazeDevicePreview gazeDevice)
    {
        TrackerState.Text = gazeDevice.ConfigurationState.ToString();
        return (gazeDevice.CanTrackEyes &&
                    gazeDevice.ConfigurationState == 
                    GazeDeviceConfigurationStatePreview.Ready);
    }
```

5. A continuación, se configuran los controladores de eventos de mirada.

    Mostramos y ocultamos la elipse de rastreo en `GazeEntered` y `GazeExited` , respectivamente.

    En `GazeMoved` , se mueve nuestra elipse de rastreo en función del [EyeGazePosition](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazepointpreview.eyegazeposition) proporcionado por el [CurrentPoint](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeenteredprevieweventargs.currentpoint) de [GazeEnteredPreviewEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeenteredprevieweventargs). También se administra el temporizador de enfoque de mirada en el [RadialProgressBar](https://docs.microsoft.com/windows/communitytoolkit/controls/radialprogressbar), que desencadena la reposición de la barra de progreso. Consulte el paso siguiente para obtener más información.

    ```csharp
    /// <summary>
    /// GazeEntered handler.
    /// </summary>
    /// <param name="sender">Source of the gaze entered event</param>
    /// <param name="e">Event args for the gaze entered event</param>
    private void GazeEntered(
        GazeInputSourcePreview sender, 
        GazeEnteredPreviewEventArgs args)
    {
        // Show ellipse representing gaze point.
        eyeGazePositionEllipse.Visibility = Visibility.Visible;

        // Mark the event handled.
        args.Handled = true;
    }

    /// <summary>
    /// GazeExited handler.
    /// Call DisplayRequest.RequestRelease to conclude the 
    /// RequestActive called in GazeEntered.
    /// </summary>
    /// <param name="sender">Source of the gaze exited event</param>
    /// <param name="e">Event args for the gaze exited event</param>
    private void GazeExited(
        GazeInputSourcePreview sender, 
        GazeExitedPreviewEventArgs args)
    {
        // Hide gaze tracking ellipse.
        eyeGazePositionEllipse.Visibility = Visibility.Collapsed;

        // Mark the event handled.
        args.Handled = true;
    }

    /// <summary>
    /// GazeMoved handler translates the ellipse on the canvas to reflect gaze point.
    /// </summary>
    /// <param name="sender">Source of the gaze moved event</param>
    /// <param name="e">Event args for the gaze moved event</param>
    private void GazeMoved(GazeInputSourcePreview sender, GazeMovedPreviewEventArgs args)
    {
        // Update the position of the ellipse corresponding to gaze point.
        if (args.CurrentPoint.EyeGazePosition != null)
        {
            double gazePointX = args.CurrentPoint.EyeGazePosition.Value.X;
            double gazePointY = args.CurrentPoint.EyeGazePosition.Value.Y;

            double ellipseLeft = 
                gazePointX - 
                (eyeGazePositionEllipse.Width / 2.0f);
            double ellipseTop = 
                gazePointY - 
                (eyeGazePositionEllipse.Height / 2.0f) - 
                (int)Header.ActualHeight;

            // Translate transform for moving gaze ellipse.
            TranslateTransform translateEllipse = new TranslateTransform
            {
                X = ellipseLeft,
                Y = ellipseTop
            };

            eyeGazePositionEllipse.RenderTransform = translateEllipse;

            // The gaze point screen location.
            Point gazePoint = new Point(gazePointX, gazePointY);

            // Basic hit test to determine if gaze point is on progress bar.
            bool hitRadialProgressBar = 
                DoesElementContainPoint(
                    gazePoint, 
                    GazeRadialProgressBar.Name, 
                    GazeRadialProgressBar); 

            // Use progress bar thickness for visual feedback.
            if (hitRadialProgressBar)
            {
                GazeRadialProgressBar.Thickness = 10;
            }
            else
            {
                GazeRadialProgressBar.Thickness = 4;
            }

            // Mark the event handled.
            args.Handled = true;
        }
    }
    ```
6. Por último, estos son los métodos que se usan para administrar el temporizador de enfoque de miración para esta aplicación.

    `DoesElementContainPoint`comprueba si el puntero de mira está sobre la barra de progreso. Si es así, inicia el temporizador de miras e incrementa la barra de progreso en cada paso del temporizador de miras.

    `SetGazeTargetLocation`establece la ubicación inicial de la barra de progreso y, si la barra de progreso se completa (según el temporizador de enfoque de miras), mueve la barra de progreso a una ubicación aleatoria.

    ```csharp
    /// <summary>
    /// Return whether the gaze point is over the progress bar.
    /// </summary>
    /// <param name="gazePoint">The gaze point screen location</param>
    /// <param name="elementName">The progress bar name</param>
    /// <param name="uiElement">The progress bar UI element</param>
    /// <returns></returns>
    private bool DoesElementContainPoint(
        Point gazePoint, string elementName, UIElement uiElement)
    {
        // Use entire visual tree of progress bar.
        IEnumerable<UIElement> elementStack = 
            VisualTreeHelper.FindElementsInHostCoordinates(gazePoint, uiElement, true);
        foreach (UIElement item in elementStack)
        {
            //Cast to FrameworkElement and get element name.
            if (item is FrameworkElement feItem)
            {
                if (feItem.Name.Equals(elementName))
                {
                    if (!timerStarted)
                    {
                        // Start gaze timer if gaze over element.
                        timerGaze.Start();
                        timerStarted = true;
                    }
                    return true;
                }
            }
        }

        // Stop gaze timer and reset progress bar if gaze leaves element.
        timerGaze.Stop();
        GazeRadialProgressBar.Value = 0;
        timerStarted = false;
        return false;
    }

    /// <summary>
    /// Tick handler for gaze focus timer.
    /// </summary>
    /// <param name="sender">Source of the gaze entered event</param>
    /// <param name="e">Event args for the gaze entered event</param>
    private void TimerGaze_Tick(object sender, object e)
    {
        // Increment progress bar.
        GazeRadialProgressBar.Value += 1;

        // If progress bar reaches maximum value, reset and relocate.
        if (GazeRadialProgressBar.Value == 100)
        {
            SetGazeTargetLocation();
        }
    }

    /// <summary>
    /// Set/reset the screen location of the progress bar.
    /// </summary>
    private void SetGazeTargetLocation()
    {
        // Ensure the gaze timer restarts on new progress bar location.
        timerGaze.Stop();
        timerStarted = false;

        // Get the bounding rectangle of the app window.
        Rect appBounds = Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().VisibleBounds;

        // Translate transform for moving progress bar.
        TranslateTransform translateTarget = new TranslateTransform();

        // Calculate random location within gaze canvas.
            Random random = new Random();
            int randomX = 
                random.Next(
                    0, 
                    (int)appBounds.Width - (int)GazeRadialProgressBar.Width);
            int randomY = 
                random.Next(
                    0, 
                    (int)appBounds.Height - (int)GazeRadialProgressBar.Height - (int)Header.ActualHeight);

        translateTarget.X = randomX;
        translateTarget.Y = randomY;

        GazeRadialProgressBar.RenderTransform = translateTarget;

        // Show progress bar.
        GazeRadialProgressBar.Visibility = Visibility.Visible;
        GazeRadialProgressBar.Value = 0;
    }
    ```

## <a name="see-also"></a>Consulte también

### <a name="resources"></a>Recursos

- [Biblioteca de miras de Windows Community Toolkit](https://docs.microsoft.com/windows/communitytoolkit/gaze/gazeinteractionlibrary)

### <a name="topic-samples"></a>Ejemplos del tema

- [Miras de ejemplo (Basic) (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-gazeinput-basic.zip)
