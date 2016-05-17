---
author: Karl-Bridge-Microsoft
Description: Además de usar los comandos de voz en Cortana para acceder a las funciones del sistema, también puedes usar los comandos de voz a través de Cortana para iniciar una aplicación en primer plano y especificar una acción o un comando para que se ejecute dentro de la aplicación.
title: Iniciar una aplicación en primer plano con los comandos de voz en Cortana
ms.assetid: 8D3D1F66-7D17-4DD1-B426-DCCBD534EF00
label: Cortana-Launch a foreground app
template: detail.hbs
---

# Activar una aplicación en primer plano con los comandos de voz a través de Cortana

Además de usar los comandos de voz en **Cortana** para acceder a las características del sistema, también puedes ampliar **Cortana** con características y funcionalidades de tu aplicación. Mediante los comandos de voz, puedes activar la aplicación en primer plano y especificar una acción o un comando para que se ejecute dentro de la aplicación. 

**API importantes**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD elements and attributes v1.2 (Elementos y atributos de VCD v1.2)**](https://msdn.microsoft.com/library/windows/apps/dn706593)


Cuando una aplicación controla un comando de voz en primer plano, esta toma el foco y se descarta a Cortana. Si lo prefieres, puedes activar la aplicación y ejecutar un comando como una tarea en segundo plano. En este caso, Cortana conserva el foco y la aplicación devuelve todos los comentarios y los resultados a través del lienzo de **Cortana** y la voz **Cortana**.

Los comandos de voz que requieren contexto o entrada de usuario adicionales (por ejemplo, enviar un mensaje a un determinado contacto) se administran mejor en una aplicación en primer plano, mientras que los comandos básicos (por ejemplo, a enumeración de próximos viajes) pueden controlarse en **Cortana** a través de una aplicación en segundo plano.

Si quieres activar una aplicación en segundo plano mediante los comandos de voz, consulta el tema [Activar una aplicación en segundo plano con los comandos de voz a través de Cortana](launch-a-background-app-with-voice-commands-in-cortana.md).

> **Nota**  
> Un comando de voz es una expresión única con un determinado propósito; esta se define en un archivo de definición de comando de voz (VCD) y se dirige a una aplicación instalada mediante **Cortana**.

> Un archivo VCD define uno o más comandos de voz, cada uno de ellos con un único propósito.

> Una definición de comando de voz puede variar en complejidad. Puede admitir desde una única expresión restringida a una colección de expresiones más flexibles y de lenguaje natural, que denoten el mismo propósito.

Para demostrar las características de una aplicación en primer plano, usaremos la aplicación de administración y planeación de viajes llamada **Adventure Works** de la [muestra de comando de voz de Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899).

Para crear un nuevo viaje en **Adventure Works** sin **Cortana**, el usuario deberá iniciar la aplicación e ir a la página **Nuevo viaje**. Para ver un viaje existente, un usuario debe iniciar la aplicación, navegar a la página **Viajes próximos** y, después, seleccionar el viaje.

Con los comandos de voz y **Cortana**, el usuario puede simplemente decir: "Adventure Works agrega un viaje" o "Agregar un viaje en Adventure Works" para iniciar la aplicación y navegar a la página **Nuevo viaje**. A su vez, al indicar "Adventure Works, muestra mi viaje a Londres", se iniciará la aplicación y navegarás a la página **Viajes próximos** que se muestra aquí.

![Inicio de una aplicación en primer plano con Cortana](images/cortana-foreground-with-adventureworks.png)

A continuación detallamos los pasos básicos para agregar funciones de comandos de voz e integrar Cortana en tu aplicación mediante entradas de voz o de teclado:

1.  Crea un archivo VCD. Se trata de un documento XML en el que se definen todos los comandos de voz que el usuario puede usar para iniciar acciones o invocar comandos al activar la aplicación. Consulta [**VCD elements and attributes v1.2 (Elementos y atributos de VCD v1.2)**](https://msdn.microsoft.com/library/windows/apps/dn706593).
2.  Registra los conjuntos de comandos en el archivo VCD cuando se inicia la aplicación.
3.  Administra el comando de activación por voz, la navegación dentro de la aplicación y la ejecución del comando.

**Requisitos previos:  **

Si acabas de empezar a desarrollar aplicaciones para la Plataforma universal de Windows (UWP), consulta estos temas para familiarizarte con las tecnologías que te presentamos aquí.

-   [Crear tu primera aplicación](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Encontrarás más información acerca de los eventos, en [Introducción a eventos y eventos enrutados](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Directrices sobre la experiencia del usuario:  **

Consulta el artículo [Directrices para el diseño de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233) para obtener información sobre cómo integrar tu aplicación con **Cortana**, y el artículo [Directrices para el diseño de voz](https://msdn.microsoft.com/library/windows/apps/dn596121) para obtener sugerencias prácticas y diseñar una aplicación habilitada para la voz que sea útil y atractiva.

## <span id="Create_a_new_solution_with_project_in_Visual_Studio"></span><span id="create_a_new_solution_with__project_in_visual_studio"></span><span id="CREATE_A_NEW_SOLUTION_WITH__PROJECT_IN_VISUAL_STUDIO"></span>Crear una nueva solución mediante un proyecto en Visual Studio


1.  Inicia Microsoft Visual Studio 2015.

    Aparecerá la página de inicio de Visual Studio 2015.

2.  En el menú **Archivo**, selecciona **Nuevo** > **Proyecto**.

    Se abre el cuadro de diálogo **Nuevo proyecto**. El panel izquierdo del cuadro de diálogo te permite seleccionar el tipo de plantillas que se muestran.

3.  En el panel izquierdo, expande **Instalado > Plantillas > Visual C\# > Windows** y, a continuación, selecciona el grupo de plantillas **Universal**. El panel central del cuadro de diálogo muestra una lista de plantillas de proyecto para aplicaciones de la Plataforma universal de Windows (UWP).
4.  En el panel central, selecciona la plantilla **Aplicación vacía (Windows universal)**.

    La plantilla **Aplicación vacía** crea una aplicación para UWP mínima que se compila y ejecuta, pero que no contiene datos ni controles de interfaz de usuario. Después, a lo largo de este curso, agregarás controles a la aplicación.

5.  En el cuadro de texto **Nombre**, escribe el nombre del proyecto. Para este ejemplo, usamos "AdventureWorks".
6.  Haz clic en **Aceptar** para crear el proyecto.

    Microsoft Visual Studio crea tu proyecto y lo muestra en el **Explorador de soluciones**.

## <span id="Add_image_assets_to_project_and_specify_them_in_the_app_manifest"></span><span id="add_image_assets_to_project_and_specify_them_in_the_app_manifest"></span><span id="ADD_IMAGE_ASSETS_TO_PROJECT_AND_SPECIFY_THEM_IN_THE_APP_MANIFEST"></span>Agregar activos de imagen al proyecto y especificarlos en el manifiesto de la aplicación
      
Las aplicaciones para UWP pueden seleccionar automáticamente las imágenes más adecuada en función de la configuración específica y las funcionalidades del dispositivo (contraste alto, píxeles efectivos, configuración regional, etc.). Lo único que tienes que hacer es proporcionar las imágenes y asegurarte de que usas la nomenclatura y organización de carpeta apropiadas en el proyecto de aplicación para las diferentes versiones de recursos. Si no proporcionas las versiones de recurso recomendadas, pueden verse afectadas la calidad de imagen, localización y accesibilidad, según las preferencias del usuario, capacidades, tipo de dispositivo y ubicación.

Para obtener más información sobre recursos de imagen para los factores de escala y contraste alto, consulta las [directrices sobre los activos de icono y de mosaicos](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets).

Debes asignar el nombre a los recursos mediante calificadores. Los calificadores de recursos son modificadores de carpetas y nombres de archivo que identifican el contexto en el que debe usarse una versión particular de un recurso.

La convención de nomenclatura estándar es `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext`. Por ejemplo: puede hacerse referencia a `images/en-US/logo.scale-100_contrast-white.png` en el código mediante la carpeta raíz y el nombre de archivo `images/logo.png`. Consulta [Cómo asignar un nombre a los recursos mediante calificadores](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324.aspx).

Te recomendamos que marques el idioma predeterminado en los archivos de recursos de cadena (como `en-US\resources.resw`), y el factor de escala predeterminado en las imágenes (como `logo.scale-100.png`); incluso si no tienes previsto ofrecer recursos localizados o de resolución múltiple. Sin embargo, como mínimo, te recomendamos que proporciones activos para los factores de escala 100, 200 y 400.

> *Importante

> El icono de la aplicación usado en el área de título del lienzo de **Cortana** es el icono Square44x44Logo especificado en el archivo "Package.appxmanifest". 
    
## <span id="Create_a_VCD_file"></span><span id="create_a_vcd_file"></span><span id="CREATE_A_VCD_FILE"></span>Crear un archivo VCD

1. En Visual Studio, haz clic con el botón secundario en el nombre del proyecto principal y selecciona **Agregar > Nuevo elemento**. Agrega un **archivo XML**.
2. Escribe un nombre para el archivo [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) (en este ejemplo es "AdventureWorksCommands.xml") y haz clic en Agregar. 
3. En el **Explorador de soluciones**, selecciona el archivo [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593).
4.  En la ventana **Propiedades**, establece **Acción de compilación** en **Contenido** y, a continuación, establece **Copiar en el directorio de salida** en **Copiar si es posterior**.

## <span id="Edit_the_VCD_file"></span><span id="edit_the_vcd_file"></span><span id="EDIT_THE_VCD_FILE"></span>Modificar el archivo VCD


Agrega un elemento **VoiceCommands** al cual apunte un atributo **xmlns**.

2. Por cada idioma que admita la aplicación, debes crear un elemento [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) que contenga los comandos de voz que admita la aplicación.

  Se pueden declarar varios elementos [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331), cada uno con un atributo [**XML: lang**](https://msdn.microsoft.com/library/windows/apps/dn722331) atributo para que tu aplicación se use en diferentes mercados. Por ejemplo, una aplicación para los Estados Unidos puede tener un [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) para inglés y otro [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) para español.

  >  **Precaución**  
  Para activar una aplicación e iniciar una acción mediante un comando de voz, la aplicación debe registrar un archivo VCD que contenga un [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) con un idioma que coincida con el idioma de la voz que el usuario seleccionó en su dispositivo. El idioma de voz se encuentra en **Configuración > Sistema > Voz > Idioma de voz**.

3. Agrega un elemento **Command** en cada comando que quieras admitir.

  Cada elemento **Command** que hayas declarado en un archivo [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) debe incluir esta información:

  - Un atributo **Name** que la aplicación usa para identificar el comando de voz en tiempo de ejecución. 
  - Un elemento **Example** que contenga una frase que describa cómo puede invocar el comando un usuario. **Cortana** muestra este ejemplo cuando el usuario dice "¿Qué puedo decir?", "Ayuda", o si presiona **Ver más**    
  -   Un elemento **ListenFor** que contenga las palabras o frases que la aplicación reconoce como un comando. Cada elemento **ListenFor** puede contener referencias a uno o más elementos **PhraseList** que contengan palabras específicas apropiadas para el comando.
  > **Nota**  
Los elementos  **ListenFor** no se pueden modificar mediante programación. Sin embargo, los elementos **PhraseList** que están asociados a elementos **ListenFor** se pueden modificar mediante programación. Las aplicaciones deben modificar el contenido del elemento **PhraseList** en tiempo de ejecución, según el conjunto de datos que se genera cuando el usuario usa la aplicación. Consulta [Modificar listas de frases de la definición de comando de voz (VCD) de forma dinámica](dynamically-modify-voice-command-definition--vcd--phrase-lists.md).

  -   Un elemento **Feedback** que contenga el texto que **Cortana** muestra y dice cuando se inicia la aplicación.

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

  A continuación, se pasa este objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) a [**InstallCommandDefinitionsFromStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn708205).    
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
}
```

## <span id="Handle_activation_and_execute_voice_commands"></span><span id="handle_activation_and_execute_voice_commands"></span><span id="HANDLE_ACTIVATION_AND_EXECUTE_VOICE_COMMANDS"></span>Controlar la activación y ejecutar los comandos de voz

Especifica la manera en que tu aplicación responde a activaciones ulteriores de comandos de voz (siempre y cuando se haya iniciado al menos una vez y tengas instalados los conjuntos de comandos de voz).

1.  Comprobar que la aplicación se haya activado mediante un comando de voz.

    Invalida el evento [**Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) y comprueba si la interfaz [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727).[**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) forma parte de la enumeración [**VoiceCommand**](https://msdn.microsoft.com/library/windows/apps/br224693).

2.  Determinar el nombre del comando y lo que se ha dicho.

    Obtén una referencia a un objeto [**VoiceCommandActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn609755) de [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727) y consulta la propiedad [**Result**](https://msdn.microsoft.com/library/windows/apps/dn609758) de un objeto [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432).

    Para determinar lo que dijo el usuario, comprueba el valor del elemento [**Text**](https://msdn.microsoft.com/library/windows/apps/dn631441) o de las propiedades semánticas de la frase reconocida en el diccionario [**SpeechRecognitionSemanticInterpretation**](https://msdn.microsoft.com/library/windows/apps/dn631443).

3.  Realizar la acción apropiada en la aplicación como, por ejemplo, ir a la página que quieras.

Para este ejemplo, volvemos a hacer referencia al archivo VCD del paso 3: Modificar el archivo VCD.

Cuando recibamos el resultado del reconocimiento de voz para el comando de voz, obtendremos el nombre del comando a partir del primer valor de la matriz [**RulePath**](https://msdn.microsoft.com/library/windows/apps/dn631438). Como el archivo VCD definió más de un posible comando de voz, es necesario comparar el valor con los nombres de comandos en el VCD y llevar a cabo la acción apropiada.

La acción más común que una aplicación puede realizar es navegar a una página con contenido relevante para el contexto del comando de voz. En este ejemplo, iremos a una página de **TripPage** e indicaremos el valor del comando de voz, cómo ha sido la entrada del comando y la frase de "destino" reconocida (si procede). Como alternativa, la aplicación podría enviar un parámetro de navegación a la clase [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432), al ir a la página.

Asimismo, puedes saber si el comando de voz que inició la aplicación se dijo realmente o si se ha escrito como texto en el diccionario [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) mediante la clave **commandMode**. El valor de dicha clave puede ser "voz" o "texto". Si el valor de la clave es "voz", te recomendamos que uses la síntesis de voz ([**Windows.Media.SpeechSynthesis**](https://msdn.microsoft.com/library/windows/apps/dn278951)) en la aplicación, para ofrecer al usuario comentarios hablados.

Si quieres saber qué tipo de contenido se dijo en la restricción **PhraseList** o **PhraseTopic** de un elemento **ListenFor**, usa la propiedad [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445). La clave del diccionario es el valor del atributo **Label** del elemento **PhraseList** o **PhraseTopic**. Aquí te mostramos cómo puedes acceder al valor de la frase **{destination}**.

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
 

 






<!--HONumber=May16_HO2-->


