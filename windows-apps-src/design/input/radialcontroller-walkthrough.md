---
ms.assetid: ''
title: Compatibilidad con Surface dial (y otros dispositivos de rueda) en la aplicación de Windows
description: Un tutorial paso a paso para agregar compatibilidad con Surface dial (y otros dispositivos de rueda) a la aplicación de Windows.
keywords: dial, radial, tutorial
ms.date: 03/11/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 74bb75fb6bced451daeb6f03fba78636d0998cec
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970280"
---
# <a name="tutorial-support-the-surface-dial-and-other-wheel-devices-in-your-windows-app"></a>Tutorial: compatibilidad con el marcado de Surface (y otros dispositivos de rueda) en la aplicación de Windows

![Imagen de Surface Dial con Surface Studio](images/radialcontroller/dial-pen-studio-600px.png)  
*Marque Surface con Surface Studio y el lápiz de Surface* (disponible para su compra en el [Microsoft Store](https://www.microsoft.com/store/d/Surface-Dial/925R551SKTGN?icid=Surface_Accessories_ModB_Surface_Dial_103116)).

En este tutorial se explica cómo personalizar las experiencias de interacción del usuario que admiten los dispositivos de rueda, como el marcado de Surface. Usamos fragmentos de código de una aplicación de ejemplo, que puede descargar de GitHub (consulte el [código de ejemplo](#sample-code)) para demostrar las distintas características y las API de [**RadialController**](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) asociadas que se describen en cada paso.

Nos centramos en lo siguiente:
* Especificar qué herramientas integradas se muestran en el menú [**RadialController**](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller)
* Agregar una herramienta personalizada al menú
* Controlar comentarios hápticos
* Personalizar las interacciones de clic
* Personalización de interacciones de rotación

Para obtener más información sobre la implementación de estas y otras características, consulte [interacciones de marcado de Surface en aplicaciones de Windows](windows-wheel-interactions.md).

## <a name="introduction"></a>Introducción

Surface dial es un dispositivo de entrada secundario que ayuda a los usuarios a ser más productivos cuando se usan junto con un dispositivo de entrada principal, como el lápiz, el toque o el mouse. Como dispositivo de entrada secundario, el marcado se usa normalmente con la mano no dominante para proporcionar acceso a los comandos del sistema y a otras herramientas y funcionalidades más contextuales. 

El marcado admite tres gestos básicos: 
- Mantenga presionado para mostrar el menú integrado de comandos.
- Gire para resaltar un elemento de menú (si el menú está activo) o para modificar la acción actual en la aplicación (si el menú no está activo).
- Haga clic para seleccionar el elemento de menú resaltado (si el menú está activo) o para invocar un comando en la aplicación (si el menú no está activo).

## <a name="prerequisites"></a>Prerrequisitos

* Un equipo (o una máquina virtual) que ejecute Windows 10 Creators Update o una versión más reciente
* [Visual Studio 2019](https://developer.microsoft.com/windows/downloads)
* [SDK de Windows 10 (10.0.15063.0)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Un dispositivo de rueda (solo el [marcado de Surface](https://www.microsoft.com/store/d/Surface-Dial/925R551SKTGN?icid=Surface_Accessories_ModB_Surface_Dial_103116) en este momento)
* Si no está familiarizado con el desarrollo de aplicaciones de aplicaciones de Windows con Visual Studio, consulte estos temas antes de empezar este tutorial:  
    * [Prepárate](https://docs.microsoft.com/windows/uwp/get-started/get-set-up)
    * [Creación de una aplicación "Hello, world" (XAML)](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)

## <a name="set-up-your-devices"></a>Configuración de los dispositivos

1. Asegúrese de que el dispositivo Windows esté encendido.
2. Vaya a **Inicio**, seleccione **configuración** > **dispositivos** > **Bluetooth & otros dispositivos**y, a continuación, Active **Bluetooth** .
3. Quite la parte inferior del dial de Surface para abrir el compartimiento de la batería y asegúrese de que haya dos baterías AAA dentro.
4. Si la pestaña batería está presente en la parte inferior del dial, quítela.
5. Mantenga presionado el pequeño botón de bajorrelieve junto a las baterías hasta que parpadee la luz de Bluetooth.
6. Vuelva a su dispositivo Windows y seleccione **Agregar Bluetooth u otro dispositivo**.
7. En el cuadro de diálogo **Agregar un dispositivo** , seleccione**marcado de Surface**de **Bluetooth** > . Ahora, su marca de Surface dial debe conectarse y agregarse a la lista de dispositivos en **mouse, teclado & lápiz** en la página de configuración de **Bluetooth & otros dispositivos** .
8. Pruebe el marcado presionando y manteniéndolo presionado durante unos segundos para mostrar el menú integrado.
9. Si el menú no se muestra en la pantalla (el marcado también debe vibrar), vuelva a la configuración de Bluetooth, quite el dispositivo y vuelva a intentar la conexión del dispositivo.

> [!NOTE]
> Los dispositivos de rueda se pueden configurar a través de la configuración de la **rueda** :
> 1. En el menú **Inicio** , seleccione **configuración**.
> 2. Seleccione **dispositivos** > **rueda**.    
> ![Pantalla de configuración de la rueda](images/radialcontroller/wheel-settings.png)

Ya está listo para iniciar este tutorial. 

## <a name="sample-code"></a>Código de ejemplo
En este tutorial, usamos una aplicación de ejemplo para mostrar los conceptos y la funcionalidad que se describen.

Descargue este ejemplo de Visual Studio y el código fuente de [GitHub](https://github.com/) en el [ejemplo Windows-appsample-Get-Started-radialcontroller](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-RadialController):

1. Seleccione el botón **clonar o descargar** verde.  
![Clonar el repositorio](images/radialcontroller/wheel-clone.png)
2. Si tiene una cuenta de GitHub, puede clonar el repositorio en el equipo local. para ello, elija **abrir en Visual Studio**. 
3. Si no tiene una cuenta de GitHub, o simplemente quiere una copia local del proyecto, elija **download zip** (tendrá que comprobar de nuevo con regularidad para descargar las actualizaciones más recientes).

> [!IMPORTANT]
> La mayor parte del código del ejemplo se marca como comentario. A medida que avanzamos en cada paso de este tema, se le pedirá que quite las marcas de comentario de las distintas secciones del código. En Visual Studio, solo tiene que resaltar las líneas de código y presionar CTRL-K y, a continuación, CTRL + U.

## <a name="components-that-support-wheel-functionality"></a>Componentes que admiten la funcionalidad de rueda

Estos objetos proporcionan la mayor parte de la experiencia del dispositivo de rueda para las aplicaciones de Windows.

| Componente | Descripción |
| --- | --- |
| [Clase **RadialController** ](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialController) y relacionada | Representa un dispositivo de entrada de rueda o accesorio, como el marcado de Surface. |
| [**IRadialControllerConfigurationInterop**](https://docs.microsoft.com/previous-versions/windows/desktop/api/radialcontrollerinterop/nn-radialcontrollerinterop-iradialcontrollerconfigurationinterop) / [**IRadialControllerInterop**](https://docs.microsoft.com/previous-versions/windows/desktop/api/radialcontrollerinterop/nn-radialcontrollerinterop-iradialcontrollerinterop)<br/>Aquí no tratamos esta funcionalidad. para obtener más información, consulte el [ejemplo de escritorio clásico de Windows](https://github.com/Microsoft/Windows-classic-samples/tree/master/Samples/RadialController). | Habilita la interoperabilidad con una aplicación de Windows. |

## <a name="step-1-run-the-sample"></a>Paso 1: ejecutar el ejemplo

Después de descargar la aplicación de ejemplo RadialController, compruebe que se ejecuta:
1. Abra el proyecto de ejemplo en Visual Studio.
2. Establezca la lista desplegable **plataformas de solución** en una selección que no sea de ARM.
3. Presione F5 para compilar, implementar y ejecutar. 

> [!NOTE]
> También **puede seleccionar** > el elemento de menú Depurar**iniciar depuración** o seleccionar el botón ejecutar **equipo local** que se ![muestra aquí: botón compilar proyecto de Visual Studio](images/radialcontroller/wheel-vsrun.png)

La ventana de la aplicación se abre y, después de que aparezca una pantalla de presentación durante unos segundos, verá esta pantalla inicial.

![Aplicación vacía](images/radialcontroller/wheel-app-step1-empty.png)

Bien, ahora tenemos la aplicación básica de Windows que usaremos en el resto de este tutorial. En los pasos siguientes, se agrega la funcionalidad **RadialController** .

## <a name="step-2-basic-radialcontroller-functionality"></a>Paso 2: funcionalidad básica de RadialController

Con la aplicación en ejecución y en primer plano, mantenga presionado el marcado de Surface para mostrar el menú de **RadialController** .

Todavía no hemos realizado ninguna personalización de nuestra aplicación, por lo que el menú contiene un conjunto predeterminado de herramientas contextuales. 

Estas imágenes muestran dos variaciones del menú predeterminado. (Hay muchos otros, entre los que se incluyen solo las herramientas básicas del sistema cuando el escritorio de Windows está activo y no hay ninguna aplicación en primer plano, herramientas de entrada de lápiz adicionales cuando está presente un control InkToolbar y herramientas de asignación cuando se usa la aplicación Maps.

| Menú RadialController (valor predeterminado)  | Menú RadialController (predeterminado con reproducción multimedia)  |
|---|---|
| ![Menú RadialController predeterminado](images/radialcontroller/wheel-app-step2-basic-default.png) | ![Menú RadialController predeterminado con música](images/radialcontroller/wheel-app-step2-basic-withmusic.png) |

Ahora comenzaremos con una personalización básica.

## <a name="step-3-add-controls-for-wheel-input"></a>Paso 3: agregar controles para la entrada de la rueda

En primer lugar, vamos a agregar la interfaz de usuario para nuestra aplicación:

1. Abra el archivo MainPage_Basic. Xaml.
2. Busque el código marcado con el título de este paso ("\<!--paso 3: agregar controles para la entrada de la rueda-->").
3. Quite las marcas de comentario de las líneas siguientes.

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
En este momento, solo están habilitados el botón **inicializar ejemplo** , el control deslizante y el modificador de alternancia. Los demás botones se usan en pasos posteriores para agregar y quitar elementos de menú **RadialController** que proporcionan acceso al control deslizante y al modificador de alternancia.

![Interfaz de usuario de la aplicación de ejemplo básica](images/radialcontroller/wheel-app-step3-basicui.png)

## <a name="step-4-customize-the-basic-radialcontroller-menu"></a>Paso 4: personalizar el menú de RadialController básico

Ahora vamos a agregar el código necesario para habilitar el acceso de **RadialController** a nuestros controles.

1. Abra el archivo MainPage_Basic. Xaml. cs.
2. Busque el código marcado con el título de este paso ("//paso 4: personalización del menú de RadialController básico").
3. Quite las marcas de comentario de las líneas siguientes:
    - Las referencias de tipo [Windows. UI. Input](https://docs.microsoft.com/uwp/api/windows.ui.input) y [Windows. Storage. streams](https://docs.microsoft.com/uwp/api/windows.storage.streams) se utilizan para la funcionalidad en los pasos siguientes:  
    
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

    - Aquí, se especifica el controlador de **clic** para el botón que habilita nuestros controles e inicializa el elemento de menú **RadialController** personalizado.

        ```csharp
        InitializeSampleButton.Click += (sender, args) =>
        { InitializeSample(sender, args); };
        ``` 

    - A continuación, se inicializa el objeto [RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) y se configuran los controladores para los eventos [RotationChanged](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationChanged) y [ButtonClicked](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ButtonClicked) .

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

    - Aquí inicializamos el elemento de menú RadialController personalizado. Usamos [CreateForCurrentView](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.CreateForCurrentView) para obtener una referencia a nuestro objeto [RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) , establecemos la sensibilidad de la rotación en "1" mediante el uso de la propiedad [RotationResolutionInDegrees](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationResolutionInDegrees) , después creamos nuestro [RadialControllerMenuItem](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem) mediante [CreateFromFontGlyph](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem.CreateFromFontGlyph), agregamos el elemento de menú a la colección de elementos de menú **RadialController** y, por último, usamos [SetDefaultMenuItems](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollerconfiguration.setdefaultmenuitems) para borrar los elementos de menú predeterminados y dejar solo nuestra herramienta personalizada. 

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
4. Ahora, vuelva a ejecutar la aplicación.
5. Seleccione el botón **inicializar controlador radial** .  
6. Con la aplicación en primer plano, mantenga presionado el marcado de Surface para mostrar el menú. Tenga en cuenta que todas las herramientas predeterminadas se han quitado (mediante el método **RadialControllerConfiguration. SetDefaultMenuItems** ), y solo se mantiene la herramienta personalizada. Este es el menú con nuestra herramienta personalizada. 

| Menú RadialController (personalizado)  | 
|---|
| ![Menú RadialController personalizado](images/radialcontroller/wheel-app-step3-custom.png) |

7. Seleccione la herramienta personalizada y pruebe las interacciones que ahora se admiten a través de Surface dial:
    * Una acción de giro mueve el control deslizante. 
    * Un clic establece el control de alternancia en activado o desactivado.

Bien, vamos a enlazar esos botones.

## <a name="step-5-configure-menu-at-runtime"></a>Paso 5: configurar el menú en tiempo de ejecución

En este paso, se enlazan los botones de menú **Agregar o quitar elemento** y **restablecer RadialController** para mostrar cómo se puede personalizar dinámicamente el menú.

1. Abra el archivo MainPage_Basic. Xaml. cs.
2. Busque el código marcado con el título de este paso ("//paso 5: configurar menú en tiempo de ejecución").
3. Quite los comentarios del código en los métodos siguientes y vuelva a ejecutar la aplicación, pero no seleccione ningún botón (guárdelo en el paso siguiente).  

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
4. Seleccione el botón **quitar elemento** y, a continuación, mantenga presionado el botón para volver a mostrar el menú.

    Observe que el menú ahora contiene la colección predeterminada de herramientas. Recuerde que, en el paso 3, al configurar el menú personalizado, hemos quitado todas las herramientas predeterminadas y hemos agregado solo nuestra herramienta personalizada. También hemos observado que, cuando el menú se establece en una colección vacía, se restablecen los elementos predeterminados para el contexto actual. (Hemos agregado nuestra herramienta personalizada antes de quitar las herramientas predeterminadas).

5. Seleccione el botón **Agregar elemento** y, a continuación, mantenga presionado el dial.

    Tenga en cuenta que el menú ahora contiene la colección predeterminada de herramientas y nuestra herramienta personalizada.

6. Seleccione el botón de **menú restablecer RadialController** y, a continuación, mantenga presionado el dial.

    Observe que el menú vuelve a su estado original.

## <a name="step-6-customize-the-device-haptics"></a>Paso 6: personalizar los hápticos del dispositivo
El marcado de Surface y otros dispositivos de rueda pueden proporcionar a los usuarios comentarios hápticos correspondientes a la interacción actual (basada en un clic o giro).

En este paso, se muestra cómo se pueden personalizar los comentarios hápticos asociando nuestros controles deslizantes y modificadores de alternancia y usando para especificar dinámicamente el comportamiento de los comentarios hápticos. En este ejemplo, el modificador Toggle debe estar establecido en ON para que se habilite feedback, mientras que el valor Slider especifica la frecuencia con que se repiten los comentarios de clic. 

> [!NOTE]
> El usuario puede deshabilitar los comentarios hápticos en la página de **configuración** >  **Devices** > **rueda** de dispositivos.

1. Abra el archivo App.xaml.cs.
2. Busque el código marcado con el título de este paso ("paso 6: personalizar el dispositivo hápticos").
3. Comente la primera y la tercera línea ("MainPage_Basic" y "MainPage") y quite la marca de comentario del segundo ("MainPage_Haptics").  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
4. Abra el archivo MainPage_Haptics. Xaml.
5. Busque el código marcado con el título de este paso ("\<!--paso 6: personalizar el dispositivo hápticos-->").
6. Quite las marcas de comentario de las líneas siguientes. (Este código de la interfaz de usuario simplemente indica qué características de hápticos admite el dispositivo actual).    

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
7. Abra el archivo MainPage_Haptics. Xaml. cs.
8. Busque el código marcado con el título de este paso ("paso 6: personalización de hápticos")
9. Quite las marcas de comentario de las líneas siguientes:  

    - La referencia de tipo [Windows. Devices. hápticos](https://docs.microsoft.com/uwp/api/windows.devices.haptics) se usa para la funcionalidad en pasos posteriores.  
    
        ```csharp
        using Windows.Devices.Haptics;
        ```

    - Aquí, se especifica el controlador para el evento [ControlAcquired](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ControlAcquired) que se desencadena cuando se selecciona nuestro elemento de menú **RadialController** personalizado.

        ```csharp
        radialController.ControlAcquired += (rc_sender, args) =>
        { RadialController_ControlAcquired(rc_sender, args); };
        ``` 

    - A continuación, definimos el controlador [ControlAcquired](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ControlAcquired) , donde deshabilitaremos los comentarios hápticos predeterminados e inicializaremos la interfaz de usuario de hápticos.

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

    - En los controladores de eventos [RotationChanged](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationChanged) y [ButtonClicked](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ButtonClicked) , se conectan los controles de botón de control deslizante y botón de alternancia correspondientes a los hápticos personalizados. 

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
    - Por último, obtenemos la **[onda](https://docs.microsoft.com/uwp/api/windows.devices.haptics.simplehapticscontrollerfeedback.Waveform)** solicitada (si se admite) para los comentarios hápticos. 

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

Ahora vuelva a ejecutar la aplicación para probar los hápticos personalizados cambiando el valor del control deslizante y cambie el estado del modificador.

## <a name="step-7-define-on-screen-interactions-for-surface-studio-and-similar-devices"></a>Paso 7: definir interacciones en pantalla para Surface Studio y dispositivos similares
Junto con Surface Studio, el marcado de Surface puede proporcionar una experiencia de usuario incluso más distintiva. 

Además de la experiencia predeterminada del menú de pulsar y sostener, Surface Dial también puede colocarse directamente sobre la pantalla de Surface Studio. Esto activa un menú especial "en pantalla". 

Al detectar la ubicación del contacto y los límites del marcado de Surface, el sistema controla la oclusión por el dispositivo y muestra una versión más grande del menú que se ajusta alrededor del exterior del marcado. Esta misma información también puede usarla la aplicación para adaptar la interfaz de usuario tanto a la presencia del dispositivo y su uso anticipado, como a la ubicación de la mano y el brazo del usuario. 

El ejemplo que acompaña a este tutorial incluye un ejemplo ligeramente más complejo en el que se muestran algunas de estas capacidades.

Para ver esto en acción (necesitará una Surface Studio):

1. Descargar el ejemplo en un dispositivo de Surface Studio (con Visual Studio instalado)
2. Abrir el ejemplo en Visual Studio
3. Abra el archivo App.xaml.cs
4. Busque el código marcado con el título de este paso ("paso 7: definir interacciones en pantalla para Surface Studio y dispositivos similares")
5. Comente la primera y la segunda línea ("MainPage_Basic" y "MainPage_Haptics") y quite la marca de comentario del tercer ("MainPage")  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
6. Ejecute la aplicación y coloque el marcado de Surface en cada una de las dos regiones de control, alternando entre ellas.    
![RadialController en pantalla](images/radialcontroller/wheel-app-step5-onscreen2.png) 

    Este es un vídeo de este ejemplo en acción:  

    <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Programming-the-Microsoft-Surface-Dial/player" width="600" height="400" allowFullScreen frameBorder="0"></iframe>  

## <a name="summary"></a>Resumen

Enhorabuena, ha completado el tutorial de introducción *: compatibilidad con Surface dial (y otros dispositivos de rueda) en la aplicación de Windows*. Le mostramos el código básico necesario para admitir un dispositivo de rueda en las aplicaciones de Windows y cómo proporcionar algunas de las experiencias de usuario más enriquecidas admitidas por las API de **RadialController** .

## <a name="related-articles"></a>Artículos relacionados

[Interacciones de Surface Dial](windows-wheel-interactions.md)

### <a name="api-reference"></a>Referencia de API

- [Clase **RadialController**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialController)
- [Clase **RadialControllerButtonClickedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerButtonClickedEventArgs)
- [Clase **RadialControllerConfiguration**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerConfiguration) 
- [Clase **RadialControllerControlAcquiredEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerControlAcquiredEventArgs) 
- [Clase **RadialControllerMenu**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerMenu) 
- [Clase **RadialControllerMenuItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerMenuItem) 
- [Clase **RadialControllerRotationChangedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerRotationChangedEventArgs) 
- [Clase **RadialControllerScreenContact**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerScreenContact) 
- [Clase **RadialControllerScreenContactContinuedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerScreenContactContinuedEventArgs) 
- [Clase **RadialControllerScreenContactStartedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs)
- [Enumeración **RadialControllerMenuKnownIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerMenuKnownIcon) 
- [Enumeración **RadialControllerSystemMenuItemKind**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerSystemMenuItemKind) 

### <a name="samples"></a>Ejemplos

#### <a name="topic-samples"></a>Ejemplos del tema

[Personalización de RadialController](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-radialcontroller-customization.zip)

#### <a name="other-samples"></a>Otros ejemplos
[Ejemplo de libro de color](https://github.com/Microsoft/Windows-appsample-coloringbook)

[Muestras de la Plataforma universal de Windows (C# y C++)](https://github.com/Microsoft/Windows-universal-samples/tree/b78d95134ce2d57c848e0a8dc339fc362748fb9c/Samples/RadialController)

[Muestra de escritorio clásico de Windows](https://github.com/Microsoft/Windows-classic-samples/tree/master/Samples/RadialController)
