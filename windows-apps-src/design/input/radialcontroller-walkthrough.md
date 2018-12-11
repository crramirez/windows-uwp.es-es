---
ms.assetid: ''
title: Compatibilidad con Surface Dial (y otros dispositivos de rueda) en tu aplicación para UWP
description: Un tutorial paso a paso para agregar compatibilidad con Surface Dial (y otros dispositivos de rueda) en tu aplicación para UWP.
keywords: dial, radial, tutorial
ms.date: 01/25/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d8729826c2f372b3d3b5607ce828aaf515e47f3d
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8882307"
---
# <a name="tutorial-support-the-surface-dial-and-other-wheel-devices-in-your-uwp-app"></a>Tutorial: Compatibilidad con Surface Dial (y otros dispositivos de rueda) en tu aplicación para UWP

![Imagen de Surface Dial con Surface Studio](images/radialcontroller/dial-pen-studio-600px.png)  
*Surface Dial con Surface Studio y Lápiz para Surface* (disponible para su compra en [Microsoft Store](https://aka.ms/purchasesurfacedial)).

En este tutorial se indica cómo personalizar las experiencias de interacción del usuario compatibles con dispositivos de rueda como Surface Dial. Usamos fragmentos de código de una aplicación de muestra, que puedes descargar de GitHub (consulta [Código de muestra](#sample-code)), para mostrar las diferentes características y las API de [**RadialController**](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) asociadas que se explican en cada paso.

Nos centraremos en lo siguiente:
* Especificación de las herramientas integradas que se muestran en el menú [**RadialController**](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller)
* Adición de una herramienta personalizada al menú
* Control de comentarios hápticos
* Personalización de las interacciones de hacer clic
* Personalización de las interacciones de rotación

Para más información sobre cómo implementar estas y otras características, consulta [Interacciones de Surface Dial en aplicaciones para UWP](windows-wheel-interactions.md).

## <a name="introduction"></a>Introducción

Surface Dial es un dispositivo de entrada secundario que ayuda a los usuarios a ser más productivos cuando se usa junto con un dispositivo de entrada principal como un lápiz, la función táctil o el mouse. Como dispositivo de entrada secundario, el Dial se usa por lo general con la mano no dominante para proporcionar acceso a los comandos del sistema, así como a otras herramientas y funcionalidad más contextuales. 

El Dial es compatible con tres gestos básicos: 
- Mantenerlo pulsado para mostrar el menú de comandos integrados.
- Girar para resaltar un elemento de menú (si el menú está activo) o para modificar la acción actual en la aplicación (si el menú no está activo).
- Hacer clic para seleccionar el elemento de menú resaltado (si el menú está activo) o para invocar un comando de la aplicación (si el menú no está activo).

## <a name="prerequisites"></a>Requisitos previos

* Un ordenador (o máquina virtual) que ejecute Windows 10 Creators Update o posterior
* [Visual Studio 2017 (10.0.15063.0)](https://developer.microsoft.com/windows/downloads)
* [Windows 10 SDK (10.0.15063.0)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Un dispositivo de rueda (solo [Surface Dial](https://aka.ms/purchasesurfacedial) en este momento)
* Si no estás familiarizado con el desarrollo de aplicaciones para la Plataforma universal de Windows (UWP) con Visual Studio, echa un vistazo a estos temas antes de empezar este tutorial:  
    * [Preparación](https://docs.microsoft.com/windows/uwp/get-started/get-set-up)
    * [Crear una aplicación "Hola mundo" (XAML)](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)

## <a name="set-up-your-devices"></a>Configurar los dispositivos

1. Asegúrate de que tu dispositivo Windows está activado.
2. Ve a **Inicio**, selecciona **Configuración** > **Dispositivos** > **Bluetooth y otros dispositivos** y, a continuación, activa **Bluetooth**.
3. Quita la parte inferior de Surface Dial para abrir el compartimento de la batería y asegúrate de que haya dos baterías AAA dentro.
4. Si la pestaña de la batería está presente en la parte inferior del Dial, quítala.
5. Mantén presionado el pequeño botón en bajorrelieve junto a las baterías hasta que la luz de Bluetooth parpadee.
6. Vuelve a tu dispositivo Windows y selecciona **Agregar Bluetooth u otro dispositivo**.
7. En el cuadro de diálogo **Agregar un dispositivo**, selecciona **Bluetooth** > **Surface Dial**. Ahora tu Surface Dial debería conectarse y agregarse a la lista de dispositivos de **Mouse, teclado y lápiz** en la página de configuración de **Bluetooth y otros dispositivos**.
8. Para probar el Dial, mantenlo presionado durante unos segundos para ver el menú integrado.
9. Si el menú no se muestra en la pantalla (el Dial también debe Vibrar), ve a la configuración de Bluetooth, quita el dispositivo e intenta volver a conectar el dispositivo.

> [!NOTE]
> Los dispositivos de rueda pueden configurarse con la configuración **Rueda**:
> 1. En el menú **Inicio**, selecciona **Configuración**.
> 2. Selecciona **Dispositivos** > **Rueda**.    
> ![Pantalla de configuración de la rueda](images/radialcontroller/wheel-settings.png)

Ya estás listo para empezar este tutorial. 

## <a name="sample-code"></a>Código de muestra
En este tutorial, usamos una aplicación de muestra para mostrar los conceptos y la funcionalidad analizada.

Descarga este código de muestra y origen de Visual Studio de [GitHub](https://github.com/) en [windows-appsample-get-started-radialcontroller sample](https://aka.ms/appsample-radialcontroller):

1. Selecciona el botón verde **Clone or download**.  
![Clonar el repositorio](images/radialcontroller/wheel-clone.png)
2. Si tienes una cuenta de GitHub, puedes clonar el repositorio en el equipo local seleccionando **Abrir en Visual Studio**. 
3. Si no tienes una cuenta de GitHub, o solo quieres una copia local del proyecto, elige **Download ZIP** (tendrás que irlo comprobando con regularidad para descargar las actualizaciones más recientes).

> [!IMPORTANT]
> La mayoría del código del ejemplo comentado. Conforme vayamos siguiendo cada paso de este tema, se te pedirá que quites los comentarios de diversas secciones del código. En Visual Studio, simplemente resalta las líneas de código y presiona CTRL-K y, a continuación, CTRL-U.

## <a name="components-that-support-wheel-functionality"></a>Componentes compatibles con la funcionalidad de rueda

Estos objetos proporcionan la mayor parte de la experiencia de dispositivo de rueda para aplicaciones para UWP.

| Componente | Descripción |
| --- | --- |
| [Clase **RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) y relacionadas | Representa un dispositivo o accesorio de entrada de rueda como Surface Dial. |
| [**IRadialControllerConfigurationInterop**](https://msdn.microsoft.com/library/windows/desktop/mt790709) / [**IRadialControllerInterop**](https://msdn.microsoft.com/library/windows/desktop/mt790711)<br/>Esta funcionalidad no se explica aquí, para más información, consulta [Muestra de escritorio clásico de Windows](https://aka.ms/radialcontrollerclassicsample). | Habilita la interoperabilidad con una aplicación para UWP. |

## <a name="step-1-run-the-sample"></a>Paso 1: ejecutar la muestra

Después de descargar la aplicación de muestra RadialController, comprueba que se ejecuta:
1. Abre el proyecto de muestra en Visual Studio.
2. Establece la lista desplegable **Plataformas de solución** en una selección que no sea ARM.
3. Presiona F5 para compilar, implementar y ejecutar. 

> [!NOTE]
> Como alternativa, puedes seleccionar el elemento de menú **Depurar** > **Iniciar depuración** o seleccionar el botón de ejecución del **Equipo local** que se muestra aquí: ![botón Proyecto de compilación de Visual Studio](images/radialcontroller/wheel-vsrun.png)

Se abre la ventana de la aplicación y, después de que aparezca una pantalla de presentación durante unos segundos, verás esta pantalla inicial.

![Aplicación vacía](images/radialcontroller/wheel-app-step1-empty.png)

Bien, ahora tenemos la aplicación para UWP básica que usaremos en el resto de este tutorial. En los pasos siguientes, agregaremos la funcionalidad **RadialController**.

## <a name="step-2-basic-radialcontroller-functionality"></a>Paso 2: funcionalidad RadialController básica

Con la aplicación en ejecución en primer plano, mantén presionado Surface Dial para mostrar el menú **RadialController**.

Aún no hemos personalizado la aplicación, por lo que el menú contiene un conjunto predeterminado de las herramientas contextuales. 

En estas imágenes se muestran dos variaciones del menú predeterminado. Hay muchas otras, que incluyen solo las herramientas básicas del sistema cuando está activo el escritorio de Windows y ninguna aplicación en primer plano, las herramientas de entrada de lápiz adicional cuando está presente un elemento InkToolbar y las herramientas de asignación cuando se usa la aplicación Mapas.

| Menú RadialController (predeterminado)  | Menú RadialController (predeterminado con reproducción multimedia)  |
|---|---|
| ![Menú RadialController predeterminado](images/radialcontroller/wheel-app-step2-basic-default.png) | ![Menú RadialController predeterminado con música](images/radialcontroller/wheel-app-step2-basic-withmusic.png) |

Ahora empezaremos a realizar alguna personalización básica.

## <a name="step-3-add-controls-for-wheel-input"></a>Paso 3: agregar controles para la entrada de rueda

Primero, vamos a agregar la interfaz de usuario para la aplicación:

1. Abre el archivo MainPage_Basic.xaml.
2. Busca el código marcado con el título "\<!-- Step 3: Add controls for wheel input -->".
3. Quita las marcas de comentario de las siguientes líneas.

    ```xaml
    <Button x:Name="InitializeSampleButton" 
            HorizontalAlignment="Center" 
            Margin="10" 
            Content="Initialize sample" />
    <ToggleButton x:Name="AddRemoveToggleButton"
                    HorizontalAlignment="Center" 
                    Margin="10" 
                    Content="Remove Item"
                    IsChecked="True" 
                    IsEnabled="False"/>
    <Button x:Name="ResetControllerButton" 
            HorizontalAlignment="Center" 
            Margin="10" 
            Content="Reset RadialController menu" 
            IsEnabled="False"/>
    <Slider x:Name="RotationSlider" Minimum="0" Maximum="10"
            Width="300"
            HorizontalAlignment="Center"/>
    <TextBlock Text="{Binding ElementName=RotationSlider, Mode=OneWay, Path=Value}"
                Margin="0,0,0,20"
                HorizontalAlignment="Center"/>
    <ToggleSwitch x:Name="ClickToggle"
                    MinWidth="0" 
                    Margin="0,0,0,20"
                    HorizontalAlignment="center"/>
    ```
En este punto, solo están habilitados el botón **Inicializar muestra**, el control deslizante y el modificador para alternar. El resto de botones se usan en pasos posteriores para agregar y quitar los elementos del menú **RadialController** que proporcionan acceso al control deslizante y al modificador para alternar.

![Interfaz de usuario de aplicación básica](images/radialcontroller/wheel-app-step3-basicui.png)

## <a name="step-4-customize-the-basic-radialcontroller-menu"></a>Paso 4: personalizar el menú RadialController básico

Ahora vamos a agregar el código necesario para habilitar el acceso de **RadialController** a los controles.

1. Abre el archivo MainPage_Basic.xaml.cs.
2. Busca el código marcado con el título "// Step 4: Basic RadialController menu customization".
3. Quita las marcas de comentario de las siguientes líneas:
    - Las referencias de tipo [Windows.UI.Input](https://docs.microsoft.com/uwp/api/windows.ui.input) y [Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/windows.storage.streams) se usan para la funcionalidad en pasos posteriores:  
    
        ```csharp
        // Using directives for RadialController functionality.
        using Windows.UI.Input;
        ```

    - Estos objetos globales ([RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller), [RadialControllerConfiguration](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollerconfiguration), [RadialControllerMenuItem](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem)) se usan en toda la aplicación.
    
        ```csharp
        private RadialController radialController;
        private RadialControllerConfiguration radialControllerConfig;
        private RadialControllerMenuItem radialControllerMenuItem;
        ```

    - Aquí especificamos el controlador **Click** para el botón que habilita nuestros controles e inicializa el elemento de menú **RadialController** personalizado.

        ```csharp
        InitializeSampleButton.Click += (sender, args) =>
        { InitializeSample(sender, args); };
        ``` 

    - A continuación, inicializamos el objeto [RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) y configuramos controladores para los eventos [RotationChanged](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationChanged) y [ButtonClicked](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ButtonClicked).

        ```csharp
        // Set up the app UI and RadialController.
        private void InitializeSample(object sender, RoutedEventArgs e)
        {
            ResetControllerButton.IsEnabled = true;
            AddRemoveToggleButton.IsEnabled = true;

            ResetControllerButton.Click += (resetsender, args) =>
            { ResetController(resetsender, args); };
            AddRemoveToggleButton.Click += (togglesender, args) =>
            { AddRemoveItem(togglesender, args); };

            InitializeController(sender, e);
        }
        ```

    - A continuación, inicializamos el elemento de menú RadialController personalizado. Usamos [CreateForCurrentView](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.CreateForCurrentView) para obtener una referencia al objeto [RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller), establecemos la sensibilidad de rotación en "1" mediante la propiedad [RotationResolutionInDegrees](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationResolutionInDegrees), después creamos un elemento [RadialControllerMenuItem](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem) con [CreateFromFontGlyph](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem.CreateFromFontGlyph), añadiremos un elemento de menú a la colección de elementos de menú **RadialController** y, por último, usamos [SetDefaultMenuItems](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollerconfiguration.setdefaultmenuitems) para borrar los elementos de menú predeterminados y dejar solo la herramienta personalizada. 

        ```csharp
        // Configure RadialController menu and custom tool.
        private void InitializeController(object sender, RoutedEventArgs args)
        {
            // Create a reference to the RadialController.
            radialController = RadialController.CreateForCurrentView();
            // Set rotation resolution to 1 degree of sensitivity.
            radialController.RotationResolutionInDegrees = 1;

            // Create the custom menu items.
            // Here, we use a font glyph for our custom tool.
            radialControllerMenuItem =
                RadialControllerMenuItem.CreateFromFontGlyph("SampleTool", "\xE1E3", "Segoe MDL2 Assets");

            // Add the item to the RadialController menu.
            radialController.Menu.Items.Add(radialControllerMenuItem);

            // Remove built-in tools to declutter the menu.
            // NOTE: The Surface Dial menu must have at least one menu item. 
            // If all built-in tools are removed before you add a custom 
            // tool, the default tools are restored and your tool is appended 
            // to the default collection.
            radialControllerConfig =
                RadialControllerConfiguration.GetForCurrentView();
            radialControllerConfig.SetDefaultMenuItems(
                new RadialControllerSystemMenuItemKind[] { });

            // Declare input handlers for the RadialController.
            // NOTE: These events are only fired when a custom tool is active.
            radialController.ButtonClicked += (clicksender, clickargs) =>
            { RadialController_ButtonClicked(clicksender, clickargs); };
            radialController.RotationChanged += (rotationsender, rotationargs) =>
            { RadialController_RotationChanged(rotationsender, rotationargs); };
        }

        // Connect wheel device rotation to slider control.
        private void RadialController_RotationChanged(
            object sender, RadialControllerRotationChangedEventArgs args)
        {
            if (RotationSlider.Value + args.RotationDeltaInDegrees >= RotationSlider.Maximum)
            {
                RotationSlider.Value = RotationSlider.Maximum;
            }
            else if (RotationSlider.Value + args.RotationDeltaInDegrees < RotationSlider.Minimum)
            {
                RotationSlider.Value = RotationSlider.Minimum;
            }
            else
            {
                RotationSlider.Value += args.RotationDeltaInDegrees;
            }
        }

        // Connect wheel device click to toggle switch control.
        private void RadialController_ButtonClicked(
            object sender, RadialControllerButtonClickedEventArgs args)
        {
            ClickToggle.IsOn = !ClickToggle.IsOn;
        }
        ```
4. Ahora, vuelve a ejecutar la aplicación.
5. Selecciona el botón **Initialize radial controlador**.  
6. Con la aplicación en primer plano, mantén presionado Surface Dial para mostrar el menú. Observa que se han quitado todas las herramientas predeterminadas (con el método **RadialControllerConfiguration.SetDefaultMenuItems**), dejando solamente la herramienta personalizada. Este es el menú con la herramienta personalizada. 

| Menú RadialController (personalizado)  | 
|---|
| ![Menú RadialController personalizado](images/radialcontroller/wheel-app-step3-custom.png) |

7. Selecciona la herramienta personalizada y prueba las interacciones que ahora son compatibles a través de Surface Dial:
    * Una acción de giro mueve el control deslizante. 
    * Un clic activa o desactiva la alternancia.

Ahora vamos a enlazar los botones.

## <a name="step-5-configure-menu-at-runtime"></a>Paso 5: configurar el menú en tiempo de ejecución

En este paso, enlazamos los botones **Add/Remove item** y **Reset RadialController menu** para mostrar cómo se puede personalizar el menú dinámicamente.

1. Abre el archivo MainPage_Basic.xaml.cs.
2. Busca el código marcado con el título "// Step 5: Configure menu at runtime".
3. Quita las marcas de comentario del código en los siguientes métodos y vuelve a ejecutar la aplicación, pero no selecciones ningún botón (lo harás en el siguiente paso).  

    ``` csharp
    // Add or remove the custom tool.
    private void AddRemoveItem(object sender, RoutedEventArgs args)
    {
        if (AddRemoveToggleButton?.IsChecked == true)
        {
            AddRemoveToggleButton.Content = "Remove item";
            if (!radialController.Menu.Items.Contains(radialControllerMenuItem))
            {
                radialController.Menu.Items.Add(radialControllerMenuItem);
            }
        }
        else if (AddRemoveToggleButton?.IsChecked == false)
        {
            AddRemoveToggleButton.Content = "Add item";
            if (radialController.Menu.Items.Contains(radialControllerMenuItem))
            {
                radialController.Menu.Items.Remove(radialControllerMenuItem);
                // Attempts to select and activate the previously selected tool.
                // NOTE: Does not differentiate between built-in and custom tools.
                radialController.Menu.TrySelectPreviouslySelectedMenuItem();
            }
        }
    }

    // Reset the RadialController to initial state.
    private void ResetController(object sender, RoutedEventArgs arg)
    {
        if (!radialController.Menu.Items.Contains(radialControllerMenuItem))
        {
            radialController.Menu.Items.Add(radialControllerMenuItem);
        }
        AddRemoveToggleButton.Content = "Remove item";
        AddRemoveToggleButton.IsChecked = true;
        radialControllerConfig.SetDefaultMenuItems(
            new RadialControllerSystemMenuItemKind[] { });
    }
    ```
4. Selecciona el botón **Quitar elemento** y, a continuación, mantén pulsado el Dial para mostrar de nuevo el menú.

    Observa que el menú contiene ahora la colección de herramientas predeterminadas. Recuerda que, en el paso 3, al configurar el menú personalizado, eliminaste todas las herramientas predeterminadas y agregaste solo la herramienta personalizada. También observamos que, cuando el menú se establece en una colección vacía, se restauran los elementos predeterminados para el contexto actual. (Agregamos la herramienta personalizada antes de quitar las herramientas predeterminadas).

5. Selecciona el botón **Agregar elemento** y, a continuación, mantén pulsado el Dial.

    Observa que el menú contiene ahora la colección de herramientas predeterminadas y la herramienta personalizada.

6. Selecciona el botón **Reset RadialController menu** y, a continuación, mantén pulsado el Dial.

    Observa que el menú vuelve a su estado original.

## <a name="step-6-customize-the-device-haptics"></a>Paso 6: personalizar el dispositivo háptico
Surface Dial y otros dispositivos de rueda pueden proporcionar a los usuarios comentarios hápticos correspondientes a la interacción actual (basada en un clic o rotación).

En este paso, te mostramos cómo personalizar los comentarios hápticos asociando el control deslizante y el modificador para alternar, y usándolos para especificar dinámicamente el comportamiento de los comentarios hápticos. Para este ejemplo, se debe activar el modificador para alternar para que los comentarios se habiliten, mientras que el valor del control deslizante especifica la frecuencia con la que se repiten los comentarios de clic. 

> [!NOTE]
> El usuario puede deshabilitar los comentarios hápticos en la página **Configuración** >  **Dispositivos** > **Rueda**.

1. Abre el archivo App.xaml.cs.
2. Busca el código marcado con el título "Step 6: Customize the device haptics".
3. Convierte en comentario la primera y tercera líneas ("MainPage_Basic" y "MainPage") y quita la marca de comentario de la segunda ("MainPage_Haptics").  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
4. Abre el archivo MainPage_Haptics.xaml.
5. Busca el código marcado con el título "\<!-- Step 6: Customize the device haptics -->".
6. Quita las marcas de comentario de las siguientes líneas. (Este código de interfaz de usuario simplemente indica qué características hápticas son compatibles con el dispositivo actual).    

    ```xaml
    <StackPanel x:Name="HapticsStack" 
                Orientation="Vertical" 
                HorizontalAlignment="Center" 
                BorderBrush="Gray" 
                BorderThickness="1">
        <TextBlock Padding="10" 
                    Text="Supported haptics properties:" />
        <CheckBox x:Name="CBDefault" 
                    Content="Default" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsChecked="True" />
        <CheckBox x:Name="CBIntensity" 
                    Content="Intensity" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPlayCount" 
                    Content="Play count" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPlayDuration" 
                    Content="Play duration" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBReplayPauseInterval" 
                    Content="Replay/pause interval" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBBuzzContinuous" 
                    Content="Buzz continuous" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBClick" 
                    Content="Click" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPress" 
                    Content="Press" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBRelease" 
                    Content="Release" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBRumbleContinuous" 
                    Content="Rumble continuous" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
    </StackPanel>
    ```
7. Abre el archivo MainPage_Haptics.xaml.cs
8. Busca el código marcado con el título "Step 6: Haptics customization".
9. Quita las marcas de comentario de las siguientes líneas:  

    - La referencia de tipo [Windows.Devices.Haptics](https://docs.microsoft.com/uwp/api/windows.devices.haptics) se usa para la funcionalidad en pasos posteriores.  
    
        ```csharp
        using Windows.Devices.Haptics;
        ```

    - Aquí especificamos el controlador para el evento [ControlAcquired](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ControlAcquired) que se desencadena cuando se selecciona el elemento de menú **RadialController** personalizado.

        ```csharp
        radialController.ControlAcquired += (rc_sender, args) =>
        { RadialController_ControlAcquired(rc_sender, args); };
        ``` 

    - A continuación, definimos el controlador [ControlAcquired](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ControlAcquired), donde deshabilitamos los comentarios hápticos predeterminados e inicializamos la interfaz de usuario háptica.

        ```csharp
        private void RadialController_ControlAcquired(
            RadialController rc_sender,
            RadialControllerControlAcquiredEventArgs args)
        {
            // Turn off default haptic feedback.
            radialController.UseAutomaticHapticFeedback = false;

            SimpleHapticsController hapticsController =
                args.SimpleHapticsController;

            // Enumerate haptic support.
            IReadOnlyCollection<SimpleHapticsControllerFeedback> supportedFeedback =
                hapticsController.SupportedFeedback;

            foreach (SimpleHapticsControllerFeedback feedback in supportedFeedback)
            {
                if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.BuzzContinuous)
                {
                    CBBuzzContinuous.IsEnabled = true;
                    CBBuzzContinuous.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Click)
                {
                    CBClick.IsEnabled = true;
                    CBClick.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Press)
                {
                    CBPress.IsEnabled = true;
                    CBPress.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Release)
                {
                    CBRelease.IsEnabled = true;
                    CBRelease.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.RumbleContinuous)
                {
                    CBRumbleContinuous.IsEnabled = true;
                    CBRumbleContinuous.IsChecked = true;
                }
            }

            if (hapticsController?.IsIntensitySupported == true)
            {
                CBIntensity.IsEnabled = true;
                CBIntensity.IsChecked = true;
            }
            if (hapticsController?.IsPlayCountSupported == true)
            {
                CBPlayCount.IsEnabled = true;
                CBPlayCount.IsChecked = true;
            }
            if (hapticsController?.IsPlayDurationSupported == true)
            {
                CBPlayDuration.IsEnabled = true;
                CBPlayDuration.IsChecked = true;
            }
            if (hapticsController?.IsReplayPauseIntervalSupported == true)
            {
                CBReplayPauseInterval.IsEnabled = true;
                CBReplayPauseInterval.IsChecked = true;
            }
        }
        ```

    - En los controladores de eventos [RotationChanged](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationChanged) y [ButtonClicked](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ButtonClicked), conectamos los controles del botón de alternancia y el control deslizante correspondientes a la háptica personalizada. 

        ```csharp
        // Connect wheel device rotation to slider control.
        private void RadialController_RotationChanged(
            object sender, RadialControllerRotationChangedEventArgs args)
        {
            ...
            if (ClickToggle.IsOn && 
                (RotationSlider.Value > RotationSlider.Minimum) && 
                (RotationSlider.Value < RotationSlider.Maximum))
            {
                SimpleHapticsControllerFeedback waveform = 
                    FindWaveform(args.SimpleHapticsController, 
                    KnownSimpleHapticsControllerWaveforms.BuzzContinuous);
                if (waveform != null)
                {
                    args.SimpleHapticsController.SendHapticFeedback(waveform);
                }
            }
        }

        private void RadialController_ButtonClicked(
            object sender, RadialControllerButtonClickedEventArgs args)
        {
            ...

            if (RotationSlider?.Value > 0)
            {
                SimpleHapticsControllerFeedback waveform = 
                    FindWaveform(args.SimpleHapticsController, 
                    KnownSimpleHapticsControllerWaveforms.Click);

                if (waveform != null)
                {
                    args.SimpleHapticsController.SendHapticFeedbackForPlayCount(
                        waveform, 1.0, 
                        (int)RotationSlider.Value, 
                        TimeSpan.Parse("1"));
                }
            }
        }
        ```
    - Por último, obtendremos el elemento **[Waveform](https://docs.microsoft.com/uwp/api/windows.devices.haptics.simplehapticscontrollerfeedback.Waveform)** (si es compatible) para los comentarios hápticos. 

        ```csharp
        // Get the requested waveform.
        private SimpleHapticsControllerFeedback FindWaveform(
            SimpleHapticsController hapticsController,
            ushort waveform)
        {
            foreach (var hapticInfo in hapticsController.SupportedFeedback)
            {
                if (hapticInfo.Waveform == waveform)
                {
                    return hapticInfo;
                }
            }
            return null;
        }
        ```

Ahora ejecuta la aplicación otra vez para probar la háptica personalizada cambiando el valor del control deslizante y el estado del modificador para alternar.

## <a name="step-7-define-on-screen-interactions-for-surface-studio-and-similar-devices"></a>Paso 7: definir interacciones en pantalla para Surface Studio y dispositivos similares
Combinado con Surface Studio, Surface Dial puede proporcionar una experiencia de usuario aún más distintiva. 

Además de la experiencia predeterminada del menú de pulsar y sostener, Surface Dial también puede colocarse directamente sobre la pantalla de Surface Studio. Esto activa un menú especial "en pantalla". 

Mediante la detección de la ubicación de contacto y los límites de Surface Dial, el sistema controla la oclusión por el dispositivo y muestra una versión aún más grande del menú que rodea la parte exterior del Dial. Esta misma información también puede usarla la aplicación para adaptar la interfaz de usuario tanto a la presencia del dispositivo y su uso anticipado, como a la ubicación de la mano y el brazo del usuario. 

La muestra que acompaña a este tutorial incluye un ejemplo un poco más complejo que muestra algunas de estas funcionalidades.

Para verlo en acción (necesitarás Surface Studio):

1. Descarga la muestra en un dispositivo Surface Studio (con Visual Studio instalado)
2. Abre de muestra en Visual Studio.
3. Abre el archivo App.xaml.cs.
4. Busca el código marcado con el título "Step 7: Define on-screen interactions for Surface Studio and similar devices".
5. Convierte en comentario la primera y segunda líneas ("MainPage_Basic" y "MainPage_Haptics") y quita la marca de comentario de la tercera("MainPage").  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
6. Ejecuta la aplicación y coloca Surface Dial en cada una de las dos regiones de control, alternando entre ellas.    
![RadialController en pantalla](images/radialcontroller/wheel-app-step5-onscreen2.png) 

    Este es un vídeo de esta muestra en acción:  

    <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Programming-the-Microsoft-Surface-Dial/player" width="600" height="400" allowFullScreen frameBorder="0"></iframe>  

## <a name="summary"></a>Resumen

Enhorabuena, has completado el *Tutorial de introducción: Compatibilidad con Surface Dial (y otros dispositivos de rueda) en tu aplicación para UWP*! Te hemos mostramos el código básico necesario para la compatibilidad con un dispositivo de rueda en tus aplicaciones para UWP y cómo proporcionar algunas de las mejores experiencias de usuario compatibles con las API de **RadialController**.
