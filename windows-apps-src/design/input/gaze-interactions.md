---
author: Karl-Bridge-Microsoft
ms.author: kbridge
title: Interacciones de mirada
Description: Learn how to design and optimize your UWP apps to provide the best experience possible for users who rely on gaze input from eye and head trackers.
label: Gaze interactions
template: detail.hbs
keywords: mirada, seguimiento de ojos, seguimiento de cabeza, punto de mirada, entrada, interacción del usuario, accesibilidad, facilidad de uso
ms.date: 05/01/2018
ms.topic: article
pm-contact: Jake Cohen
dev-contact: Austin Hodges
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: de3c1875063d40daa2ad9cfb490dfb00fd0bddd2
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5642745"
---
# <a name="gaze-interactions-and-eye-tracking-in-uwp-apps"></a>Interacciones de mirada y seguimiento de ojos en aplicaciones para UWP

![Zona destacada de seguimiento de ojos](images/gaze/eyecontrolbanner1.png)

Proporciona soporte técnico para realizar un seguimiento de la mirada del usuario, atención y presencia en función de la ubicación y el movimiento de sus ojos.

> [!NOTE]
> Para la entrada de mirada en [Windows Mixed Reality](https://docs.microsoft.com/windows/mixed-reality/), consulta [Mirada](https://docs.microsoft.com/windows/mixed-reality/gaze).

**API importantes**: [Windows.Devices.Input.Preview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview), [GazeDevicePreview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicepreview), [GazePointPreview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazepointpreview), [GazeInputSourcePreview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview)

## <a name="overview"></a>Introducción

La entrada de mirada es un modo eficaz de interactuar y usar aplicaciones de Windows y UWP que es especialmente útil como una tecnología de asistencia para usuarios con enfermedades neuromusculares (por ejemplo, ELA) y otras limitaciones que implican discapacidades musculares o nerviosas.

Además, la entrada de mirada ofrece igualmente atractivas oportunidades tanto para juegos (incluida adquisición y seguimiento del objetivo) y aplicaciones de productividad tradicionales, quioscos y otros escenarios interactivos donde dispositivos de entrada tradicionales (teclado, mouse, pantalla táctil) no están disponibles, o donde podría resultar útil/de ayuda liberar manos del usuario para otras tareas (por ejemplo, sujetar bolsas de compra).

> [!NOTE]
> La compatibilidad con el hardware de seguimiento de ojos se introdujo en **Windows 10 Fall Creators Update** junto con [Control ocular](https://support.microsoft.com/en-us/help/4043921/windows-10-get-started-eye-control), una característica integrada que permite usar los ojos para controlar el puntero en pantalla, escribir con el teclado en pantalla y comunicarse con personas utilizando texto a voz. Un conjunto de [API para UWP]([Windows.Devices.Input.Preview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview)) para crear aplicaciones que pueden interactuar con el hardware de seguimiento de ojos está disponible con **Actualización de abril de 2018 de Windows 10 (versión 1803, compilación 17134)** y versiones posteriores.

## <a name="privacy"></a>Privacidad

Debido a los datos personales potencialmente sensibles recopilados por los dispositivos de seguimiento de ojos, es necesario declarar la funcionalidad `gazeInput` en el manifiesto de la aplicación para UWP (consulta la sección **Instalación** siguiente). Al declarar, Windows automáticamente pide a los usuarios mediante un cuadro de diálogo de consentimiento (cuando se ejecuta la aplicación por primera vez), donde el usuario debe conceder permiso para que la aplicación se comunique con el dispositivo de seguimiento de ojos y acceda a estos datos.

Además, si la aplicación recopila, almacena o transfiere datos de seguimiento de los ojos, debe describir esto en la declaración de privacidad de la aplicación y seguir todos los otros requisitos de **Información personal** en el [Acuerdo para desarrolladores de aplicaciones](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) y las [Directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies).

## <a name="setup"></a>Instalación

Para usar las API de entrada de mirada en la aplicación para UWP tienes que: 

- Especificar la funcionalidad `gazeInput` en el manifiesto de la aplicación.

    Abrir el archivo **Package.appxmanifest** con el diseñador de manifiestos de Visual Studio o agregar la funcionalidad manualmente seleccionando **Ver código** e insertando la siguiente `DeviceCapability` en el nodo `Capabilities`:

    ```xaml
    <Capabilities>
       <DeviceCapability Name="gazeInput" />
    </Capabilities>
    ```

- Un dispositivo de seguimiento de ojos compatible con Windows conectado al sistema (integrado o periférico) y encendido.

    Consulta [Introducción a control de ojos en Windows 10](https://support.microsoft.com/help/4043921/windows-10-get-started-eye-control#supported-devices ) para obtener una lista de los dispositivos de seguimiento de ojos compatibles.

## <a name="basic-eye-tracking"></a>Seguimiento de ojos básico

En este ejemplo, se muestra cómo realizar un seguimiento de la mirada del usuario dentro de una aplicación para UWP y usar una función de sincronización con pruebas de posicionamiento básicas para indicar en qué medida pueden mantener el enfoque de la mirada en un elemento específico.

Se usa una pequeña elipse para mostrar dónde se encuentra el punto de mirada dentro de la ventanilla de la aplicación, mientras una [RadialProgressBar](https://docs.microsoft.com/en-us/windows/uwpcommunitytoolkit/controls/radialprogressbar) desde el [Kit de herramientas de Comunidad Windows](https://docs.microsoft.com/en-us/windows/uwpcommunitytoolkit/) se coloca aleatoriamente en el lienzo. Cuando se detecta el foco de la mirada foco en la barra de progreso, se inicia un temporizador y la barra de progreso se reubica aleatoriamente en el lienzo cuando la barra de progreso llega al 100%.

![Ejemplo de seguimiento de la mirada con temporizador](images/gaze/gaze-input-timed2.gif)

*Ejemplo de seguimiento de la mirada con temporizador*

**Descargar este ejemplo de [Ejemplo de entrada de mirada (básico)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-gazeinput-basic.zip)**

1. En primer lugar, debemos configurar la interfaz de usuario (MainPage.xaml).

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

    En este fragmento de código, declaramos nuestros objetos globales e invalidamos el evento de la página [OnNavigatedTo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) para iniciar nuestro [observador de dispositivo de mirada](https://docs.microsoft.com/en-us/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview) y el evento de página [OnNavigatedFrom](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) para detener nuestro [observador de dispositivo de mirada](https://docs.microsoft.com/en-us/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview).

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

3. A continuación, agregamos nuestros métodos de observador de dispositivos de mirada. 
    
    En `StartGazeDeviceWatcher`, llamamos a [CreateWatcher](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.createwatcher) y declaramos las escuchas de eventos del dispositivo de seguimiento de ojos ([DeviceAdded](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.added), [DeviceUpdated](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.updated) y [DeviceRemoved](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.removed)).

    En `DeviceAdded`, comprobamos el estado del dispositivo de seguimiento de ojos. Si es un dispositivo viable, incrementamos el recuento de dispositivos y habilitamos el seguimiento de mirada. Consulta el paso siguiente para obtener más información.

    En `DeviceUpdated`, habilitamos el seguimiento de mirada cuando se activa este evento si se vuelve a calibrar un dispositivo.

    En `DeviceRemoved`, reducimos el contador de dispositivos y quitamos los controladores de eventos del dispositivo.

    En `StopGazeDeviceWatcher`, cerramos el observador de dispositivos de mirada. 

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

4. A continuación, comprobamos si el dispositivo es viable en `IsSupportedDevice` y, si es así, intentamos habilitar el seguimiento de miradas en `TryEnableGazeTrackingAsync`.

    En `TryEnableGazeTrackingAsync`, declaramos los controladores de eventos de mirada y llamamos a [GazeInputSourcePreview.GetForCurrentView()](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.getforcurrentview) para obtener una referencia al origen de entrada (esta debe llamarse en el subproceso de interfaz de usuario, consulta [Mantener la capacidad de respuesta de los subprocesos de la interfaz de usuario](https://docs.microsoft.com/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive)).

    > [!NOTE]
    > Debes llamar a [GazeInputSourcePreview.GetForCurrentView()](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.getforcurrentview) solo cuando esté conectado un dispositivo de seguimiento de ojos compatible y la aplicación lo requiera. De lo contrario, no es necesario el cuadro de diálogo de consentimiento.

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

5. A continuación, configuramos nuestros controladores de eventos de mirada.

    Mostramos y ocultamos la elipse de seguimiento de mirada en `GazeEntered` y `GazeExited`, respectivamente.

    En `GazeMoved`, movemos nuestra elipse de seguimiento de mirada en función de la [EyeGazePosition](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazepointpreview.eyegazeposition) proporcionada por el [CurrentPoint](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeenteredprevieweventargs.currentpoint) de los [GazeEnteredPreviewEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeenteredprevieweventargs). También administramos el temporizador de foco de mirada en la [RadialProgressBar](https://docs.microsoft.com/en-us/windows/uwpcommunitytoolkit/controls/radialprogressbar), que activa el reposicionamiento de la barra de progreso. Consulta el paso siguiente para obtener más información.

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
6. Por último, estos son los métodos usados para administrar el temporizador de foco de mirada para esta aplicación.

    `DoesElementContainPoint` comprueba si el puntero de mirada está sobre la barra de progreso. Si es así, inicia el temporizador de mirada e incrementa la barra de progreso en cada tic del temporizador de mirada.

    `SetGazeTargetLocation` establece la ubicación inicial de la barra de progreso y, si se completa la barra de progreso (según el temporizador de foco de mirada), mueve la barra de progreso a una ubicación aleatoria.

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

## <a name="see-also"></a>Puedes ver también

### <a name="resources"></a>Recursos

- [Biblioteca de miradas del kit de herramientas de Comunidad Windows](https://docs.microsoft.com/en-us/windows/uwpcommunitytoolkit/gaze/gazeinteractionlibrary)

### <a name="topic-samples"></a>Ejemplos del tema

- [Ejemplo de mirada (básico) (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-gazeinput-basic.zip)
