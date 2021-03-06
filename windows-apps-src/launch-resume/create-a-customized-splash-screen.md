---
title: Mostrar una pantalla de presentación durante más tiempo
description: Muestra una pantalla de presentación más tiempo creando una pantalla de presentación extendida para tu aplicación. Esta pantalla extendida imita la pantalla de presentación que aparece al iniciar la aplicación, pero se puede personalizar.
ms.assetid: CD3053EB-7F86-4D74-9C5A-950303791AE3
ms.date: 02/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8eaede14fad920653e24506298c1d80b7d176a1f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164979"
---
# <a name="display-a-splash-screen-for-more-time"></a>Mostrar una pantalla de presentación durante más tiempo

**API importantes**

-   [Clase SplashScreen](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen)
-   [Eventos Window.SizeChanged](/uwp/api/windows.ui.xaml.window.sizechanged)
-   [Método Application.OnLaunched](/uwp/api/windows.ui.xaml.application.onlaunched)

Muestra una pantalla de presentación más tiempo creando una pantalla de presentación extendida para tu aplicación. Esta pantalla extendida imita la pantalla de presentación que aparece al iniciar la aplicación, pero se puede personalizar. Tanto si lo que quieres es mostrar información en tiempo real sobre la carga como simplemente dar a tu aplicación un poco más de tiempo para preparar la interfaz de usuario inicial, la pantalla de presentación extendida te permite definir la experiencia de inicio.

> [!NOTE]
> La frase "pantalla de presentación extendida" de este tema hace referencia a una pantalla de presentación que permanece en la pantalla durante un largo período de tiempo. No significa una subclase que se deriva de la clase [SplashScreen](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen).

Asegúrate de que tu pantalla de presentación extendida imite la pantalla de presentación predeterminada; para ello, sigue estas recomendaciones:

-   La página de pantalla de presentación extendida debe usar una imagen de 620 x 300 píxeles que sea coherente con la imagen especificada para tu pantalla de presentación en el manifiesto de la aplicación (la imagen de la pantalla de presentación de tu aplicación). En Microsoft Visual Studio 2015, la configuración de la pantalla de presentación se almacena en la sección **Pantalla de presentación** de la pestaña **Activos visuales** en el manifiesto de la aplicación (archivo Package.appxmanifest).
-   La pantalla de presentación extendida debe usar un color de fondo que sea coherente con el color de fondo especificado para la pantalla de presentación en el manifiesto de la aplicación (el fondo de la pantalla de presentación de tu aplicación).
-   El código debe usar la clase [SplashScreen](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen) para posicionar la imagen de la pantalla de presentación de tu aplicación en las mismas coordenadas que la pantalla de presentación predeterminada.
-   El código debe responder a los eventos de cambio de tamaño de la ventana (como cuando se gira la pantalla o la aplicación se sitúa junto a otra aplicación en la pantalla) usando la clase [SplashScreen](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen) para volver a posicionar los elementos en la pantalla de presentación extendida.

Usa los siguientes pasos para crear una pantalla de presentación extendida que imite eficazmente la pantalla de presentación predeterminada.

## <a name="add-a-blank-page-item-to-your-existing-app"></a>Agregar un elemento **Página en blanco** a tu aplicación existente


En este tema se da por hecho que quieres agregar una pantalla de presentación extendida a un proyecto de aplicación Plataforma universal de Windows (UWP) mediante C#, Visual Basic o C++.

-   Abre la aplicación en Visual Studio.
-   Presiona o abre el **Proyecto** en la barra de menús y haz clic en **Agregar nuevo elemento**. Aparecerá un cuadro de diálogo **Agregar nuevo elemento**.
-   En este cuadro de diálogo, agrega una nueva **Página en blanco** a tu aplicación. En este tema, la página de la pantalla de presentación extendida se llama "ExtendedSplash".

Agregar un elemento de **Página en blanco** genera dos archivos, uno para el marcado (ExtendedSplash.xaml) y otro para el código (ExtendedSplash.xaml.cs).

## <a name="essential-xaml-for-an-extended-splash-screen"></a>XAML esencial para una pantalla de presentación extendida


Sigue estos pasos para agregar una imagen y un control de progreso a la pantalla de presentación extendida.

En el archivo ExtendedSplash.xaml:

-   Modifica la propiedad [Background](/uwp/api/windows.ui.xaml.controls.control.backgroundproperty) del elemento [Grid](/uwp/api/Windows.UI.Xaml.Controls.Grid) predeterminado para que coincida con el color de fondo establecido para la pantalla de presentación de la aplicación en el manifiesto de la aplicación (en la sección **Activos visuales** de tu archivo Package.appxmanifest). El color predeterminado de la pantalla de presentación es un gris claro (valor hexadecimal \# 464646). Ten en cuenta que este elemento **Grid** se proporciona de forma predeterminada cuando se crea una nueva **Página en blanco**. No tienes que usar **Grid**; tan solo es una base útil para crear una pantalla de presentación extendida.
-   Agrega un elemento [Canvas](/uwp/api/Windows.UI.Xaml.Controls.Canvas) a [Grid](/uwp/api/Windows.UI.Xaml.Controls.Grid). Usarás **Canvas** para posicionar la imagen de la pantalla de presentación extendida.
-   Agrega un elemento [Image](/uwp/api/Windows.UI.Xaml.Controls.Image) a [Canvas](/uwp/api/Windows.UI.Xaml.Controls.Canvas). Usa la misma imagen de 600 x 320 píxeles para la pantalla de presentación extendida que usaste para la pantalla de presentación predeterminada.
-   (Opcional) Agrega un control de progreso para mostrar a los usuarios que tu aplicación se está cargando. En este tema, se agrega [ProgressRing](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing), en lugar de un control [ProgressBar](/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) determinado o indeterminado.

En el ejemplo siguiente se muestra una [cuadrícula](/uwp/api/Windows.UI.Xaml.Controls.Grid) con estos cambios y adiciones.

```xaml
    <Grid Background="#464646">
        <Canvas>
            <Image x:Name="extendedSplashImage" Source="Assets/SplashScreen.png"/>
            <ProgressRing Name="splashProgressRing" IsActive="True" Width="20" HorizontalAlignment="Center"></ProgressRing>
        </Canvas>
    </Grid>
```

> [!NOTE]
> En este ejemplo se establece el ancho de la [ProgressRing](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) en 20 píxeles. Puedes establecer manualmente el ancho en un valor adecuado para tu aplicación; sin embargo, los controles con un ancho inferior a 20 píxeles no se representarán.

## <a name="essential-code-for-an-extended-splash-screen-class"></a>Código esencial para una clase de pantalla de presentación extendida


La pantalla de presentación extendida debe responder cuando cambie el tamaño (solo Windows) o la orientación de la ventana. La posición de la imagen que usas debe actualizarse para que tu pantalla de presentación extendida se vea correctamente sin importar cómo cambie la ventana.

Usa estos pasos para definir métodos para mostrar correctamente tu pantalla de presentación extendida.

1.  **Agregar los espacios de nombres necesarios**

    Tendrá que agregar los siguientes espacios de nombres a **ExtendedSplash.Xaml.CS** para tener acceso a la clase [SplashScreen](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen) , al struct [Rect](/uwp/api/windows.foundation.rect) y a los eventos [window. SizeChanged](/uwp/api/windows.ui.xaml.window.sizechanged) .

    ```cs
    using Windows.ApplicationModel.Activation;
    using Windows.Foundation;
    using Windows.UI.Core;
    ```

2.  **Crear una clase parcial y declarar variables de clase**

    Incluye el código siguiente en ExtendedSplash.xaml.cs para crear una clase parcial que represente una pantalla de presentación extendida.

    ```cs
    partial class ExtendedSplash : Page
    {
        internal Rect splashImageRect; // Rect to store splash screen image coordinates.
        private SplashScreen splash; // Variable to hold the splash screen object.
        internal bool dismissed = false; // Variable to track splash screen dismissal status.
        internal Frame rootFrame;

       // Define methods and constructor
    }
    ```

    Varios métodos usan estas variables de clase. La variable `splashImageRect` almacena las coordenadas en que el sistema mostró la imagen de la pantalla de presentación de la aplicación. La variable `splash` almacena un objeto [SplashScreen](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen) y la variable `dismissed` realiza un seguimiento de si se ha descartado o no la pantalla de presentación que muestra el sistema.

3.  **Definir un constructor para tu clase que posicione correctamente la imagen**

    El código siguiente define un constructor para la clase de pantalla de presentación extendida que escucha los eventos de cambio de tamaño de la ventana, posiciona la imagen y el control de progreso (opcional) en la pantalla de presentación extendida, crea un marco para la navegación y llama a un método asincrónico para restaurar un estado de sesión guardado.

    ```cs
    public ExtendedSplash(SplashScreen splashscreen, bool loadState)
    {
        InitializeComponent();

        // Listen for window resize events to reposition the extended splash screen image accordingly.
        // This ensures that the extended splash screen formats properly in response to window resizing.
        Window.Current.SizeChanged += new WindowSizeChangedEventHandler(ExtendedSplash_OnResize);

        splash = splashscreen;
        if (splash != null)
        {
            // Register an event handler to be executed when the splash screen has been dismissed.
            splash.Dismissed += new TypedEventHandler<SplashScreen, Object>(DismissedEventHandler);

            // Retrieve the window coordinates of the splash screen image.
            splashImageRect = splash.ImageLocation;
            PositionImage();

            // If applicable, include a method for positioning a progress control.
            PositionRing();
        }

        // Create a Frame to act as the navigation context
        rootFrame = new Frame();            
    }
    ```

    Asegúrate de registrar tu controlador [Window.SizeChanged](/uwp/api/windows.ui.xaml.window.sizechanged) (`ExtendedSplash_OnResize` en el ejemplo) en tu constructor de clase para que la aplicación posicione correctamente la imagen en la pantalla de presentación extendida.

4.  **Definir una clase para posicionar la imagen en la pantalla de presentación extendida**

    Este código muestra cómo posicionar la imagen en la página de pantalla de presentación extendida con la variable de clase `splashImageRect`.

    ```cs
    void PositionImage()
    {
        extendedSplashImage.SetValue(Canvas.LeftProperty, splashImageRect.X);
        extendedSplashImage.SetValue(Canvas.TopProperty, splashImageRect.Y);
        extendedSplashImage.Height = splashImageRect.Height;
        extendedSplashImage.Width = splashImageRect.Width;
    }
    ```

5.  **(Opcional) Definir un método de clase para posicionar un control de progreso en la pantalla de presentación extendida**

    Si decidiste agregar un control [ProgressRing](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) a la pantalla de presentación extendida, posiciónalo con relación a la imagen de la pantalla de presentación. Agrega el siguiente código a ExtendedSplash.xaml.cs para centrar **ProgressRing** 32 píxeles por debajo de la imagen.

    ```cs
    void PositionRing()
    {
        splashProgressRing.SetValue(Canvas.LeftProperty, splashImageRect.X + (splashImageRect.Width*0.5) - (splashProgressRing.Width*0.5));
        splashProgressRing.SetValue(Canvas.TopProperty, (splashImageRect.Y + splashImageRect.Height + splashImageRect.Height*0.1));
    }
    ```

6.  **Dentro de la clase, definir un controlador para el evento Dismissed**

    En ExtendedSplash.xaml.cs, responde cuando se produzca el evento [SplashScreen.Dismissed](/uwp/api/windows.applicationmodel.activation.splashscreen.dismissed) estableciendo la clase `dismissed` en true. Si tu aplicación tiene operaciones de configuración, agrégalas a este controlador de eventos.

    ```cs
    // Include code to be executed when the system has transitioned from the splash screen to the extended splash screen (application's first view).
    void DismissedEventHandler(SplashScreen sender, object e)
    {
        dismissed = true;

        // Complete app setup operations here...
    }
    ```

    Una vez completada la configuración de la aplicación, sal de la pantalla de presentación extendida. El siguiente código define un método denominado `DismissExtendedSplash` que navega a la `MainPage` definida en el archivo MainPage.xaml de tu aplicación.

    ```cs
    async void DismissExtendedSplash()
      {
         await Windows.ApplicationModel.Core.CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal,() =>            {
              rootFrame = new Frame();
              rootFrame.Content = new MainPage(); Window.Current.Content = rootFrame;
            });
      }
      ```

7.  **Dentro de la clase, definir un controlador para los eventos Window.SizeChanged**

    Prepara la pantalla de presentación extendida para volver a posicionar sus elementos si un usuario cambia el tamaño de la ventana. Este código responde cuando se produce un evento [Window.SizeChanged](/uwp/api/windows.ui.xaml.window.sizechanged) capturando las nuevas coordenadas y volviendo a posicionar la imagen. Si agregaste un control de progreso a la pantalla de presentación extendida, vuelve a posicionarlo dentro del controlador de eventos también.

    ```cs
    void ExtendedSplash_OnResize(Object sender, WindowSizeChangedEventArgs e)
    {
        // Safely update the extended splash screen image coordinates. This function will be executed when a user resizes the window.
        if (splash != null)
        {
            // Update the coordinates of the splash screen image.
            splashImageRect = splash.ImageLocation;
            PositionImage();

            // If applicable, include a method for positioning a progress control.
            // PositionRing();
        }
    }
    ```

    > [!NOTE]
    > Antes de intentar obtener la ubicación de la imagen, asegúrese de que la variable de clase ( `splash` ) contiene un objeto [SplashScreen](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen) válido, como se muestra en el ejemplo.

     

8.  **(Opcional) Agregar un método de clase para restaurar un estado de sesión guardado**

    El código que agregaste al método [OnLaunched](/uwp/api/windows.ui.xaml.application.onlaunched) en el paso 4: [Modificar el controlador de activación de inicio](#modify-the-launch-activation-handler) hace que la aplicación muestre una pantalla de presentación extendida cuando se inicia. Para consolidar todos los métodos relacionados con el inicio de la aplicación en la clase de pantalla de presentación extendida, puede considerar la posibilidad de agregar un método al archivo ExtendedSplash.xaml.cs para restaurar el estado de la aplicación.

    ```cs
    void RestoreState(bool loadState)
    {
        if (loadState)
        {
             // code to load your app's state here
        }
    }
    ```

    Cuando modifiques el controlador de activación del inicio en App.xaml.cs, establece también `loadstate` en true si el anterior [ApplicationExecutionState](/uwp/api/Windows.ApplicationModel.Activation.ApplicationExecutionState) de la aplicación era **Terminated**. En este caso, el método `RestoreState` restaura la aplicación a su estado anterior. Para obtener una introducción al inicio, suspensión y finalización de aplicaciones, consulta [Ciclo de vida de la aplicación](app-lifecycle.md).

## <a name="modify-the-launch-activation-handler"></a>Modificar el controlador de activación de inicio


Cuando se inicia tu aplicación, el sistema pasa la información de la pantalla de presentación al controlador de eventos de activación de inicio de la aplicación. Puedes usar esta información para posicionar correctamente la imagen en la página de la pantalla de presentación extendida. Puedes obtener esta información de la pantalla de presentación en los argumentos del evento de activación que se pasan al controlador [OnLaunched](/uwp/api/windows.ui.xaml.application.onlaunched) de tu aplicación (consulta la variable `args` en el código siguiente).

Si aún no has invalidado el controlador [OnLaunched](/uwp/api/windows.ui.xaml.application.onlaunched) para tu aplicación, consulta [Ciclo de vida de la aplicación](app-lifecycle.md) para aprender a controlar los eventos de activación.

En App.xaml.cs, agrega el código siguiente para crear y mostrar una pantalla de presentación extendida.

```cs
protected override void OnLaunched(LaunchActivatedEventArgs args)
{
    if (args.PreviousExecutionState != ApplicationExecutionState.Running)
    {
        bool loadState = (args.PreviousExecutionState == ApplicationExecutionState.Terminated);
        ExtendedSplash extendedSplash = new ExtendedSplash(args.SplashScreen, loadState);
        Window.Current.Content = extendedSplash;
    }

    Window.Current.Activate();
}
```

## <a name="complete-code"></a>Código completo

El código siguiente difiere ligeramente de los fragmentos de código mostrados en los pasos anteriores.
-   ExtendedSplash.xaml incluye un botón `DismissSplash`. Al hacer clic en este botón, el controlador de eventos `DismissSplashButton_Click` llama al método `DismissExtendedSplash`. En la aplicación, llama a `DismissExtendedSplash` cuando la aplicación haya terminado de cargar los recursos o de inicializar su interfaz de usuario.
-   Esta aplicación también usa una plantilla de proyecto de aplicación para UWP que usa la navegación [Frame](/uwp/api/Windows.UI.Xaml.Controls.Frame). Como resultado, en App.xaml.cs, el controlador de activación de inicio ([OnLaunched](/uwp/api/windows.ui.xaml.application.onlaunched)) define un `rootFrame` y lo usa para establecer el contenido de la ventana de la aplicación.

### <a name="extendedsplashxaml"></a>ExtendedSplash. Xaml

En este ejemplo se incluye un `DismissSplash` botón porque no tiene recursos de la aplicación para cargar. En tu aplicación, descarta la pantalla de presentación extendida automáticamente cuando la aplicación haya terminado de cargar los recursos o de preparar la interfaz de usuario inicial.

```xml
<Page
    x:Class="SplashScreenExample.ExtendedSplash"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:SplashScreenExample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="#464646">
        <Canvas>
            <Image x:Name="extendedSplashImage" Source="Assets/SplashScreen.png"/>
            <ProgressRing Name="splashProgressRing" IsActive="True" Width="20" HorizontalAlignment="Center"/>
        </Canvas>
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Bottom">
            <Button x:Name="DismissSplash" Content="Dismiss extended splash screen" HorizontalAlignment="Center" Click="DismissSplashButton_Click" />
        </StackPanel>
    </Grid>
</Page>
```

### <a name="extendedsplashxamlcs"></a>ExtendedSplash.xaml.cs

Tenga en cuenta que `DismissExtendedSplash` se llama al método desde el controlador de eventos click para el `DismissSplash` botón. En tu aplicación, no necesitarás un botón `DismissSplash`. En su lugar, llama a `DismissExtendedSplash` cuando la aplicación haya terminado de cargar los recursos y quieras navegar a su página principal.

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;

using Windows.ApplicationModel.Activation;
using SplashScreenExample.Common;
using Windows.UI.Core;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/p/?LinkID=234238

namespace SplashScreenExample
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    partial class ExtendedSplash : Page
    {
        internal Rect splashImageRect; // Rect to store splash screen image coordinates.
        private SplashScreen splash; // Variable to hold the splash screen object.
        internal bool dismissed = false; // Variable to track splash screen dismissal status.
        internal Frame rootFrame;

        public ExtendedSplash(SplashScreen splashscreen, bool loadState)
        {
            InitializeComponent();

            // Listen for window resize events to reposition the extended splash screen image accordingly.
            // This is important to ensure that the extended splash screen is formatted properly in response to snapping, unsnapping, rotation, etc...
            Window.Current.SizeChanged += new WindowSizeChangedEventHandler(ExtendedSplash_OnResize);

            splash = splashscreen;

            if (splash != null)
            {
                // Register an event handler to be executed when the splash screen has been dismissed.
                splash.Dismissed += new TypedEventHandler<SplashScreen, Object>(DismissedEventHandler);

                // Retrieve the window coordinates of the splash screen image.
                splashImageRect = splash.ImageLocation;
                PositionImage();

                // Optional: Add a progress ring to your splash screen to show users that content is loading
                PositionRing();
            }

            // Create a Frame to act as the navigation context
            rootFrame = new Frame();

            // Restore the saved session state if necessary
            RestoreState(loadState);
        }

        void RestoreState(bool loadState)
        {
            if (loadState)
            {
                // TODO: write code to load state
            }
        }

        // Position the extended splash screen image in the same location as the system splash screen image.
        void PositionImage()
        {
            extendedSplashImage.SetValue(Canvas.LeftProperty, splashImageRect.X);
            extendedSplashImage.SetValue(Canvas.TopProperty, splashImageRect.Y);
            extendedSplashImage.Height = splashImageRect.Height;
            extendedSplashImage.Width = splashImageRect.Width;

        }

        void PositionRing()
        {
            splashProgressRing.SetValue(Canvas.LeftProperty, splashImageRect.X + (splashImageRect.Width*0.5) - (splashProgressRing.Width*0.5));
            splashProgressRing.SetValue(Canvas.TopProperty, (splashImageRect.Y + splashImageRect.Height + splashImageRect.Height*0.1));
        }

        void ExtendedSplash_OnResize(Object sender, WindowSizeChangedEventArgs e)
        {
            // Safely update the extended splash screen image coordinates. This function will be fired in response to snapping, unsnapping, rotation, etc...
            if (splash != null)
            {
                // Update the coordinates of the splash screen image.
                splashImageRect = splash.ImageLocation;
                PositionImage();
                PositionRing();
            }
        }

        // Include code to be executed when the system has transitioned from the splash screen to the extended splash screen (application's first view).
        void DismissedEventHandler(SplashScreen sender, object e)
        {
            dismissed = true;

            // Complete app setup operations here...
        }

        void DismissExtendedSplash()
        {
            // Navigate to mainpage
            rootFrame.Navigate(typeof(MainPage));
            // Place the frame in the current Window
            Window.Current.Content = rootFrame;
        }

        void DismissSplashButton_Click(object sender, RoutedEventArgs e)
        {
            DismissExtendedSplash();
        }
    }
}
```

### <a name="appxamlcs"></a>App.xaml.cs

Este proyecto se creó con la plantilla de proyecto **aplicación vacía (XAML)** de aplicación para UWP en Visual Studio. Los controladores de eventos `OnNavigationFailed` y `OnSuspending` se generan automáticamente y no es necesario cambiarlos para implementar una pantalla de presentación extendida. En este tema solo se modifica `OnLaunched`.

Si no usaste una plantilla de proyecto para tu aplicación, consulta el paso 4: [Modificar el controlador de activación de inicio](#modify-the-launch-activation-handler) para ver un ejemplo de `OnLaunched` modificado que no usa la navegación de [Frame](/uwp/api/Windows.UI.Xaml.Controls.Frame).

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.ApplicationModel;
using Windows.ApplicationModel.Activation;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;

// The Blank Application template is documented at https://go.microsoft.com/fwlink/p/?LinkID=234227

namespace SplashScreenExample
{
    /// <summary>
    /// Provides application-specific behavior to supplement the default Application class.
    /// </summary>
    sealed partial class App : Application
    {
        /// <summary>
        /// Initializes the singleton application object.  This is the first line of authored code
        /// executed, and as such is the logical equivalent of main() or WinMain().
        /// </summary>
        public App()
        {
            Microsoft.ApplicationInsights.WindowsAppInitializer.InitializeAsync(
            Microsoft.ApplicationInsights.WindowsCollectors.Metadata |
            Microsoft.ApplicationInsights.WindowsCollectors.Session);
            this.InitializeComponent();
            this.Suspending += OnSuspending;
        }

        /// <summary>
        /// Invoked when the application is launched normally by the end user.  Other entry points
        /// will be used such as when the application is launched to open a specific file.
        /// </summary>
        /// <param name="e">Details about the launch request and process.</param>
        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
#if DEBUG
            if (System.Diagnostics.Debugger.IsAttached)
            {
                this.DebugSettings.EnableFrameRateCounter = true;
            }
#endif

            Frame rootFrame = Window.Current.Content as Frame;

            // Do not repeat app initialization when the Window already has content,
            // just ensure that the window is active
            if (rootFrame == null)
            {
                // Create a Frame to act as the navigation context and navigate to the first page
                rootFrame = new Frame();
                // Set the default language
                rootFrame.Language = Windows.Globalization.ApplicationLanguages.Languages[0];

                rootFrame.NavigationFailed += OnNavigationFailed;

                //  Display an extended splash screen if app was not previously running.
                if (e.PreviousExecutionState != ApplicationExecutionState.Running)
                {
                    bool loadState = (e.PreviousExecutionState == ApplicationExecutionState.Terminated);
                    ExtendedSplash extendedSplash = new ExtendedSplash(e.SplashScreen, loadState);
                    rootFrame.Content = extendedSplash;
                    Window.Current.Content = rootFrame;
                }
            }

            if (rootFrame.Content == null)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(typeof(MainPage), e.Arguments);
            }
            // Ensure the current window is active
            Window.Current.Activate();
        }

        /// <summary>
        /// Invoked when Navigation to a certain page fails
        /// </summary>
        /// <param name="sender">The Frame which failed navigation</param>
        /// <param name="e">Details about the navigation failure</param>
        void OnNavigationFailed(object sender, NavigationFailedEventArgs e)
        {
            throw new Exception("Failed to load Page " + e.SourcePageType.FullName);
        }

        /// <summary>
        /// Invoked when application execution is being suspended.  Application state is saved
        /// without knowing whether the application will be terminated or resumed with the contents
        /// of memory still intact.
        /// </summary>
        /// <param name="sender">The source of the suspend request.</param>
        /// <param name="e">Details about the suspend request.</param>
        private void OnSuspending(object sender, SuspendingEventArgs e)
        {
            var deferral = e.SuspendingOperation.GetDeferral();
            // TODO: Save applicaiton state and stop any background activity
            deferral.Complete();
        }
    }
}
```

## <a name="related-topics"></a>Temas relacionados


* [Ciclo de vida de la aplicación](app-lifecycle.md)

**Referencia**

* [Espacio de nombres Windows.ApplicationModel.Activation](/uwp/api/Windows.ApplicationModel.Activation)
* [Clase Windows.ApplicationModel.Activation.SplashScreen](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen)
* [Propiedad Windows.ApplicationModel.Activation.SplashScreen.ImageLocation](/uwp/api/windows.applicationmodel.activation.splashscreen.imagelocation)
* [Evento Windows.ApplicationModel.Core.CoreApplicationView.Activated](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated)

 

 