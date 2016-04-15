---
Description: Además de usar los comandos de voz en Cortana para acceder a las funciones del sistema, también puedes usar los comandos de voz a través de Cortana para iniciar una aplicación en primer plano y especificar una acción o un comando para que se ejecute dentro de la aplicación.
title: Iniciar una aplicación en primer plano con los comandos de voz en Cortana
ms.assetid: 8D3D1F66-7D17-4DD1-B426-DCCBD534EF00
label: Cortana-Launch a foreground app
template: detail.hbs
---

# Activar una aplicación en primer plano con los comandos de voz a través de Cortana


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD elements and attributes v1.2 (Elementos y atributos de VCD v1.2)**](https://msdn.microsoft.com/library/windows/apps/dn706593)

Además de usar los comandos de voz en **Cortana** para acceder a las características del sistema, también puedes ampliar **Cortana** con características y funcionalidades de la aplicación. Con los comandos de voz, se puede activar la aplicación en primer plano y una acción o un comando que se ejecuta dentro de la aplicación. 

Cuando una aplicación controla un comando de voz en primer plano, esta toma el foco y se descarta Cortana. Si lo prefieres, puedes activar la aplicación y ejecutar un comando como una tarea en segundo plano. En este caso, Cortana conserva el foco y la aplicación devuelve todos los comentarios y los resultados a través del lienzo de **Cortana** y la voz **Cortana**.

Los comandos de voz que requieren contexto o entrada de usuario adicionales (por ejemplo, enviar un mensaje a un determinado contacto) se administran mejor en una aplicación en primer plano, mientras que los comandos básicos (por ejemplo, a enumeración de próximos viajes) pueden controlarse en **Cortana** a través de una aplicación en segundo plano.

Si quieres activar una aplicación en segundo plano con los comandos de voz, consulta el tema [Activar una aplicación en segundo plano con los comandos de voz a través de Cortana](launch-a-background-app-with-voice-commands-in-cortana.md).

> **Nota**  
> Un comando de voz es una única expresión con un determinado propósito, definida en un archivo de definición de comando de voz (VCD), dirigida a una aplicación instalada mediante **Cortana**.

> Un archivo VCD define uno o más comandos de voz, cada uno con un único propósito.

> Una definición de comando de voz puede variar en complejidad. Puede admitir desde una única expresión restringida a una colección de expresiones más flexibles y de lenguaje natural, que denoten el mismo propósito.

Para demostrar las funciones de una aplicación en primer plano, usaremos la administración y planificación de viajes llamada **Adventure Works** de la [Muestra de comando de voz de Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899).

Para crear un nuevo viaje de **Adventure Works** sin **Cortana**, un usuario debe iniciar la aplicación y navegar a la página **Nuevo viaje**. Para ver un viaje existente, un usuario debe iniciar la aplicación, navegar a la página **Viajes próximos** y, después, seleccionar el viaje.

Con los comandos de voz y **Cortana**, el usuario puede simplemente decir: "Adventure Works agrega un viaje" o "Agregar un viaje en Adventure Works" para iniciar la aplicación y navegar a la página **Nuevo viaje**. A su vez, al indicar "Adventure Works, muestra mi viaje a Londres", se iniciará la aplicación y navegarás a la página **Viajes próximos** que se muestra aquí.

![Inicio de una aplicación en primer plano con Cortana](images/cortana-foreground-with-adventureworks.png)

A continuación detallamos los pasos básicos para agregar funciones de comandos de voz e integrar Cortana en tu aplicación mediante entradas de voz o de teclado:

1.  Crea un archivo VCD. Se trata de un documento XML en el que se definen todos los comandos de voz que el usuario puede decir para iniciar acciones o invocar comandos al activar la aplicación. Consulta [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593).
2.  Registra los conjuntos de comandos en el archivo VCD cuando se inicia la aplicación.
3.  Administra el comando de activación por voz, la navegación dentro de la aplicación y la ejecución del comando.

**Requisitos previos:  **

Si acabas de empezar a desarrollar aplicaciones de la Plataforma Universal Windows (UWP), consulta estos temas para familiarizarte con las tecnologías presentadas aquí.

-   [Crea tu primera aplicación](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Encontrarás más información acerca de los eventos, en [Introducción a eventos y eventos enrutados](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Directrices sobre la experiencia del usuario:  **

Consulta [Directrices para el diseño de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233) para obtener información sobre cómo integrar tu aplicación con **Cortana** y [Directrices para el diseño de voz](https://msdn.microsoft.com/library/windows/apps/dn596121) para obtener sugerencias prácticas y diseñar una aplicación habilitada para la voz que sea útil y atractiva.

## <span id="Create_a_new_solution_with_project_in_Visual_Studio"></span><span id="create_a_new_solution_with__project_in_visual_studio"></span><span id="CREATE_A_NEW_SOLUTION_WITH__PROJECT_IN_VISUAL_STUDIO"></span>Crear una nueva solución con un proyecto principal en Visual Studio


1.  Inicia Microsoft Visual Studio 2015.

    Aparecerá la página de inicio de Visual Studio 2015.

2.  En el menú **Archivo**, selecciona **Nuevo** > **Proyecto**.

    Aparecerá el cuadro de diálogo **Nuevo proyecto**. El panel izquierdo del cuadro de diálogo te permite seleccionar el tipo de plantillas que se muestran.

3.  En el panel izquierdo, expande **Instalado > Plantillas > Visual C\# > Windows** y, a continuación, selecciona el grupo de plantillas **Universal**. El panel central del cuadro de diálogo muestra una lista de plantillas de proyecto para aplicaciones de la Plataforma universal de Windows (UWP).
4.  En el panel central, selecciona la plantilla **Aplicación vacía (Windows universal)**.

    La plantilla **Aplicación vacía** crea una aplicación para UWP mínima que se compila y ejecuta, pero que no contiene datos ni controles de interfaz de usuario. Después, a lo largo de este curso, agregarás controles a la aplicación.

5.  En el cuadro de texto **Nombre**, escribe el nombre del proyecto. Para este ejemplo, usamos "AdventureWorks".
6.  Haz clic en **Aceptar** para crear el proyecto.

    Microsoft Visual Studio crea tu proyecto y lo muestra en el **Explorador de soluciones**.

## <span id="Add_image_assets_to_project_and_specify_them_in_the_app_manifest"></span><span id="add_image_assets_to_project_and_specify_them_in_the_app_manifest"></span><span id="ADD_IMAGE_ASSETS_TO_PROJECT_AND_SPECIFY_THEM_IN_THE_APP_MANIFEST"></span>Agregar activos de imagen al proyecto principal y especificarlos en el manifiesto de la aplicación
      
Las aplicaciones para UWP pueden seleccionar automáticamente las imágenes más adecuada en función de la configuración específica y las funcionalidades del dispositivo (contraste alto, píxeles efectivos, configuración regional, etc.). Lo único que tienes que hacer es proporcionar las imágenes y asegurarte de que usas la nomenclatura y organización de carpeta apropiadas en el proyecto de aplicación para las diferentes versiones de recursos. Si no proporcionas las versiones de recurso recomendadas, pueden verse afectadas la calidad de imagen, localización y accesibilidad, según las preferencias del usuario, capacidades, tipo de dispositivo y ubicación.

Para más información sobre recursos de imagen para los factores de escala y contraste alto, consulta las [directrices para los activos de icono y el mosaico](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets).

Se asigna nombre a los recursos mediante calificadores. Los calificadores de recursos son modificadores de carpetas y nombres de archivo que identifican el contexto en el que debe usarse una versión particular de un recurso.

La convención de nomenclatura estándar es `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext`. Por ejemplo: `images/en-US/logo.scale-100_contrast-white.png` simplemente se denomina en código mediante la carpeta raíz y el nombre de archivo: `images/logo.png`. Consulta [Cómo asignar un nombre a los recursos mediante calificadores](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh965324.aspx).

Se recomienda que marques el idioma predeterminado en los archivos de recursos de cadena (como `en-US\resources.resw`) y el factor de escala predeterminado en las imágenes (como `logo.scale-100.png`), incluso si no tienes previstu ofrecer recursos localizados o de resolución múltiple. Sin embargo, como mínimo, te recomendamos que proporciones activos para los factores de escala 100, 200 y 400.

> *Importante

> El icono de la aplicación usado en el área de título del lienzo de **Cortana** es el icono Square44x44Logo especificado en el archivo "Package.appxmanifest". 
    
## <span id="Create_a_VCD_file"></span><span id="create_a_vcd_file"></span><span id="CREATE_A_VCD_FILE"></span>Crea un archivo VCD.

1. En Visual Studio, haz clic en el nombre del proyecto principal, selecciona **Agregar > Nuevo elemento**. Agregar un **archivo XML**.
2. Escribe un nombre para el archivo [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) (para este ejemplo, "AdventureWorksCommands.xml") y haz clic en Agregar. 
3. En el **Explorador de soluciones**, selecciona el archivo [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593).
4.  En la ventana **Propiedades**, establece **Acción de compilación** en **Contenido** y luego establece **Copiar en el directorio de salida** en **Copiar si es posterior**.

## <span id="Edit_the_VCD_file"></span><span id="edit_the_vcd_file"></span><span id="EDIT_THE_VCD_FILE"></span>Modificar el archivo VCD


. Agregar un elemento **VoiceCommands** con un atributo **xmlns** que apunte a "http://schemas.microsoft.com/voicecommands/1.2".

2. Para cada idioma admitido por la aplicación, crea un elemento [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) que contenga los comandos de voz que admite la aplicación.

  Se pueden declarar varios elementos [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331), cada uno con un atributo [**XML: lang**](https://msdn.microsoft.com/library/windows/apps/dn722331) atributo para que tu aplicación se use en diferentes mercados. Por ejemplo, una aplicación para los Estados Unidos puede tener un [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) para inglés y otro [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) para español.

  >  **Precaución**  
  Para activar una aplicación e iniciar una acción mediante un comando de voz, la aplicación debe registrar un archivo VCD que contenga un [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) con un idioma que coincida con el idioma de la voz que el usuario seleccionó en su dispositivo. El idioma de voz se encuentra en **Configuración > Sistema > Voz > Idioma de voz**.

3. Agregar un elemento **Command** para cada comando que quieras admitir.

  Cada **Command** declarado en un archivo [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) debe incluir esta información:

  - Un atributo **Name** que la aplicación usa para identificar el comando de voz en tiempo de ejecución. 
  - Un elemento **Example** que contenga una frase que describa cómo puede invocar el comando un usuario. **Cortana** muestra este ejemplo cuando el usuario dice "¿Qué puedo decir?", "Ayuda", o presiona **Ver más**.    
  -   Un elemento **ListenFor** que contiene las palabras o frases que la aplicación reconoce para iniciar como un comando. Cada elemento **ListenFor** puede contener referencias a uno o más elementos **PhraseList** que contengan palabras específicas apropiadas para el comando.
  > **Nota**  
  **Los elementos ListenFor** no se pueden modificar mediante programación. Sin embargo, los elementos **PhraseList** que están asociados a elementos **ListenFor** se pueden modificar mediante programación. Las aplicaciones deben modificar el contenido del elemento **PhraseList** en tiempo de ejecución, según el conjunto de datos que se genera cuando el usuario usa la aplicación. Consulta [Modificar de forma dinámica listas de frases de definición de comando de voz (VCD)](dynamically-modify-voice-command-definition--vcd--phrase-lists.md).

  -   Un elemento **Feedback** que contiene el texto que **Cortana** muestra y dice cuando se inicia la aplicación.

Un elemento **Navigate** indica que el comando de voz debe activar la aplicación en primer plano. En este ejemplo, el comando ```showTripToDestination``` es una tarea en primer plano.

Un elemento **VoiceCommandService** indica que el comando de voz activa la aplicación en segundo plano. El valor del atributo **Target** de este elemento debe coincidir con el valor del atriuto **Target** del elemento [**uap:AppService**](https://msdn.microsoft.com/library/windows/apps/dn934779) en el archivo package.appxmanifest. En este ejemplo, los comandos ```whenIsTripToDestination``` y ```cancelTripToDestination``` son tareas en segundo plano que especifican el nombre del servicio de aplicación como "AdventureWorksVoiceCommandService".

Para obtener más detalles, consulta la referencia [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593).

Esta es una parte del archivo [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) que define la voz en inglés para los comandos de la aplicación **Adventure Works**.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.2">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> Show trip to London </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> show [my] trip to {destination} </ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> show [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate />
    </Command>

    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Looking for trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>
    
    <Command Name="cancelTripToDestination">
      <Example> Cancel my trip to Las Vegas </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> cancel [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> cancel [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Cancelling trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>

    <PhraseList Label="destination">
      <Item>London</Item>
      <Item>Las Vegas</Item>
      <Item>Melbourne</Item>
      <Item>Yosemite National Park</Item>
    </PhraseList>
  </CommandSet>
```

## <span id="Install_the_VCD_commands"></span><span id="install_the_vcd_commands"></span><span id="INSTALL_THE_VCD_COMMANDS"></span>Instalar los comandos de VCD


La aplicación debe ejecutarse una vez para instalar el VCD. 

>  **Nota**  
No se conservan datos de comandos de voz en las instalaciones de la aplicación. Para garantizar que los datos de comandos de voz de la aplicación permanecen intactos, considera la posibilidad de inicializar el archivo VCD cada vez que la aplicación se inicie o active, o bien mantener una configuración que indique si el VCD está instalado actualmente.

En el archivo "app.xaml.cs":

1. Agrega la siguiente directiva de uso:  
```csharp
using Windows.Storage;
```
2. Marca el método "OnLaunched" con el modificador asincrónico.  
```csharp
protected async override void OnLaunched(LaunchActivatedEventArgs e)
```
3. Llama a [**InstallCommandDefinitionsFromStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn708205) en el controlador [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) para registrar los comandos de voz que el sistema debe esperar que se digan.

  En la muestra de Adventure Works, primero definimos un objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171). 

  Luego, llamamos a [**GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) para inicializarlo con nuestro archivo "AdventureWorksCommands.xml".

  Este objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) se pasa a continuación a [**InstallCommandDefinitionsFromStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn708205).    
```csharp
try
{
  // Install the main VCD. 
  StorageFile vcdStorageFile = 
    await Package.Current.InstalledLocation.GetFileAsync(
      @"AdventureWorksCommands.xml");

  await Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
    InstallCommandDefinitionsFromStorageFileAsync(vcdStorageFile);

  // Update phrase list.
  ViewModel.ViewModelLocator locator = App.Current.Resources["ViewModelLocator"] as ViewModel.ViewModelLocator;
  if(locator != null)
  {
     await locator.TripViewModel.UpdateDestinationPhraseList();
  }
}
catch (Exception ex)
{
  System.Diagnostics.Debug.WriteLine("Installing Voice Commands Failed: " + ex.ToString());
}```

## <span id="Handle_activation_and_execute_voice_commands"></span><span id="handle_activation_and_execute_voice_commands"></span><span id="HANDLE_ACTIVATION_AND_EXECUTE_VOICE_COMMANDS"></span>Handle activation and execute voice commands

Specify how your app responds to subsequent voice command activations (after it has been launched at least once and the voice command sets have been installed).

1.  Confirm that your app was activated by a voice command.

    Override the [**Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) event and check whether [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727).[**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) is [**VoiceCommand**](https://msdn.microsoft.com/library/windows/apps/br224693).

2.  Determine the name of the command and what was spoken.

    Get a reference to a [**VoiceCommandActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn609755) object from the [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727) and query the [**Result**](https://msdn.microsoft.com/library/windows/apps/dn609758) property for a [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) object.

    To determine what the user said, check the value of [**Text**](https://msdn.microsoft.com/library/windows/apps/dn631441) or the semantic properties of the recognized phrase in the [**SpeechRecognitionSemanticInterpretation**](https://msdn.microsoft.com/library/windows/apps/dn631443) dictionary.

3.  Take the appropriate action in your app, such as navigating to the desired page.

For this example, we refer back to the VCD in Step 3: Edit the VCD file.

Once we get the speech-recognition result for the voice command, we get the command name from the first value in the [**RulePath**](https://msdn.microsoft.com/library/windows/apps/dn631438) array. As the VCD file defined more than one possible voice command, we need to compare the value against the command names in the VCD and take the appropriate action.

The most common action an application can take is to navigate to a page with content relevant to the context of the voice command. For this example, we navigate to a **TripPage** page and pass in the value of the voice command, how the command was input, and the recognized "destination" phrase (if applicable). Alternatively, the app could send a navigation parameter to the [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) when navigating to the page.

You can find out whether the voice command that launched your app was actually spoken, or whether it was typed in as text, from the [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) dictionary using the **commandMode** key. The value of that key will be either "voice" or "text". If the value of the key is "voice", consider using speech synthesis ([**Windows.Media.SpeechSynthesis**](https://msdn.microsoft.com/library/windows/apps/dn278951)) in your app to provide the user with spoken feedback.

Use the [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) to find out the content spoken in the **PhraseList** or **PhraseTopic** constraints of a **ListenFor** element. The dictionary key is the value of the **Label** attribute of the **PhraseList** or **PhraseTopic** element. Here, we show how to access the value of **{destination}** phrase.

``` csharp
/// <summary>
/// Entry point for an application activated by some means other than normal launching. 
/// This includes voice commands, URI, share target from another app, and so on. 
/// 
/// NOTE:
/// A previous version of the VCD file might remain in place 
/// if you modify it and update the app through the store. 
/// Activations might include commands from older versions of your VCD. 
/// Try to handle these commands gracefully.
/// </summary>
/// <param name="args">Details about the activation method.</param>
protected override void OnActivated(IActivatedEventArgs args)
{
    base.OnActivated(args);

    Type navigationToPageType;
    ViewModel.TripVoiceCommand? navigationCommand = null;

    // Voice command activation.
    if (args.Kind == ActivationKind.VoiceCommand)
    {
        // Event args can represent many different activation types. 
        // Cast it so we can get the parameters we care about out.
        var commandArgs = args as VoiceCommandActivatedEventArgs;

        Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = commandArgs.Result;

        // Get the name of the voice command and the text spoken. 
        // See VoiceCommands.xml for supported voice commands.
        string voiceCommandName = speechRecognitionResult.RulePath[0];
        string textSpoken = speechRecognitionResult.Text;

        // commandMode indicates whether the command was entered using speech or text.
        // Apps should respect text mode by providing silent (text) feedback.
        string commandMode = this.SemanticInterpretation("commandMode", speechRecognitionResult);
        
        switch (voiceCommandName)
        {
            case "showTripToDestination":
                // Access the value of {destination} in the voice command.
                string destination = this.SemanticInterpretation("destination", speechRecognitionResult);

                // Create a navigation command object to pass to the page. 
                navigationCommand = new ViewModel.TripVoiceCommand(
                    voiceCommandName,
                    commandMode,
                    textSpoken,
                    destination);

                // Set the page to navigate to for this voice command.
                navigationToPageType = typeof(View.TripDetails);
                break;
            default:
                // If we can't determine what page to launch, go to the default entry point.
                navigationToPageType = typeof(View.TripListView);
                break;
        }
    }
    // Protocol activation occurs when a card is clicked within Cortana (using a background task).
    else if (args.Kind == ActivationKind.Protocol)
    {
        // Extract the launch context. In this case, we're just using the destination from the phrase set (passed
        // along in the background task inside Cortana), which makes no attempt to be unique. A unique id or 
        // identifier is ideal for more complex scenarios. We let the destination page check if the 
        // destination trip still exists, and navigate back to the trip list if it doesn't.
        var commandArgs = args as ProtocolActivatedEventArgs;
        Windows.Foundation.WwwFormUrlDecoder decoder = new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
        var destination = decoder.GetFirstValueByName("LaunchContext");

        navigationCommand = new ViewModel.TripVoiceCommand(
                                "protocolLaunch",
                                "text",
                                "destination",
                                destination);

        navigationToPageType = typeof(View.TripDetails);
    }
    else
    {
        // If we were launched via any other mechanism, fall back to the main page view.
        // Otherwise, we'll hang at a splash screen.
        navigationToPageType = typeof(View.TripListView);
    }

    // Repeat the same basic initialization as OnLaunched() above, taking into account whether
    // or not the app is already active.
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active.
    if (rootFrame == null)
    {
        // Create a frame to act as the navigation context and navigate to the first page.
        rootFrame = new Frame();
        App.NavigationService = new NavigationService(rootFrame);

        rootFrame.NavigationFailed += OnNavigationFailed;

        // Place the frame in the current window.
        Window.Current.Content = rootFrame;
    }

    // Since we're expecting to always show a details page, navigate even if 
    // a content frame is in place (unlike OnLaunched).
    // Navigate to either the main trip list page, or if a valid voice command
    // was provided, to the details page for that trip.
    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active
    Window.Current.Activate();
}

/// <summary>
/// Returns the semantic interpretation of a speech result. 
/// Returns null if there is no interpretation for that key.
/// </summary>
/// <param name="interpretationKey">The interpretation key.</param>
/// <param name="speechRecognitionResult">The speech recognition result to get the semantic interpretation from.</param>
/// <returns></returns>
private string SemanticInterpretation(string interpretationKey, SpeechRecognitionResult speechRecognitionResult)
{
  return speechRecognitionResult.SemanticInterpretation.Properties[interpretationKey].FirstOrDefault();
}
```

## <span id="related_topics"></span>Artículos relacionados


**Desarrolladores**
* [Interacciones de Cortana](cortana-interactions.md)
* [Definir restricciones de reconocimiento personalizadas](define-custom-recognition-constraints.md)
* [**VCD elements and attributes v1.2 (Elementos y atributos de VCD v1.2)**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Diseñadores**
* [Directrices para el diseño de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Directrices para el diseño de Voz](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Muestras**
* [Muestra de comando de voz de Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=Mar16_HO4-->


