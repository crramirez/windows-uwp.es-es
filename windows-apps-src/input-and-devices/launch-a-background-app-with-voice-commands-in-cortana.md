---
author: Karl-Bridge-Microsoft
Description: Además de usar los comandos de voz en Cortana para tener acceso a las características del sistema, también puedes ampliar Cortana con características y funcionalidades desde una aplicación en segundo plano con comandos de voz que especifiquen una acción o un comando que se debe ejecutar dentro de la aplicación.
title: Iniciar una aplicación en segundo plano con los comandos de voz en Cortana
ms.assetid: DF5B530C-57DD-4CA5-B3BE-1A0B3695C9C6
label: Launch a background app
template: detail.hbs
---

# Activar una aplicación en segundo plano con los comandos de voz a través de Cortana

Además de usar los comandos de voz en **Cortana** para tener acceso a las características del sistema, también puedes ampliar **Cortana** con características y funcionalidades desde una aplicación (como tarea en segundo plano) con comandos de voz que especifiquen una acción o un comando que se debe ejecutar. Cuando una aplicación controla un comando de voz en segundo plano, no tiene foco. En su lugar, devuelve todos los comentarios y los resultados a través del lienzo de **Cortana** y la voz de **Cortana**.

**API importantes**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD elements and attributes v1.2 (Elementos y atributos de VCD v1.2)**](https://msdn.microsoft.com/library/windows/apps/dn706593)



Las aplicaciones pueden activarse en primer plano (la aplicación recibe el foco) o en segundo plano (**Cortana** conserva el foco), según la complejidad de la interacción. Por ejemplo, los comandos de voz que requieren contexto o entrada de usuario adicionales (por ejemplo, enviar un mensaje a un determinado contacto) se administran mejor en una aplicación en primer plano, mientras que los comandos básicos (por ejemplo, a enumeración de próximos viajes) pueden controlarse en **Cortana** a través de una aplicación en segundo plano.

Si quieres activar una aplicación en primer plano con los comandos de voz, consulta el tema [Activar una aplicación en primer plano con los comandos de voz a través de Cortana](launch-a-foreground-app-with-voice-commands-in-cortana.md).

> **Nota**  
> Un comando de voz es una única expresión con un determinado propósito, definida en un archivo de definición de comando de voz (VCD), dirigida a una aplicación instalada mediante **Cortana**.

> Un archivo VCD define uno o más comandos de voz, cada uno con un único propósito.

> Una definición de comando de voz puede variar en complejidad. Puede admitir desde una única expresión restringida a una colección de expresiones más flexibles y de lenguaje natural, que denoten el mismo propósito.

Para demostrar las funciones de una aplicación en segundo plano, usaremos la administración y planificación de viajes llamada **Adventure Works** de la [Muestra de comando de voz de Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899).

Aquí tienes un resumen de la aplicación **Adventure Works** integrada con el lienzo de **Cortana**.

![introducción al lienzo de Cortana ](images/cortana-overview.png)

Para ver un viaje de **Adventure Works** sin **Cortana**, un usuario debe iniciar la aplicación y navegar a la página **Viajes próximos**.

Con los comandos de voz mediante **Cortana** para iniciar la aplicación en segundo plano, el usuario puede en su lugar decir simplemente, "Adventure Works, ¿cuándo es mi viaje a Las Vegas?". La aplicación controla el comando y **Cortana** muestra los resultados junto con el icono de la aplicación y otros datos de aplicación, si se han proporcionado. Este es un ejemplo de una consulta básica de viaje y la pantalla de resultado de **Cortana** que muestra y dice "Tu próximo viaje a Las Vegas es el 1 de agosto".

![Una consulta básica y la pantalla de resultados con la aplicación Adventure Works en segundo plano](images/cortana-backgroundapp-result.png)

Estos son los pasos básicos para agregar funciones de comandos de voz y ampliar **Cortana** con funciones en segundo plano de tu aplicación, mediante entradas de voz o de teclado:

1.  Crea un servicio de aplicaciones (consulta [**Windows.ApplicationModel.AppService**](https://msdn.microsoft.com/library/windows/apps/dn921731)) que **Cortana** invoque en segundo plano.
2.  Crea un archivo VCD. Se trata de un documento XML en el que se definen todos los comandos de voz que el usuario puede decir para iniciar acciones o invocar comandos al activar la aplicación. Consulta [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593).
3.  Registra los conjuntos de comandos en el archivo VCD cuando se inicia la aplicación.
4.  Controla la activación en segundo plano del servicio de aplicaciones y la ejecución del comando de voz.
5.  Muestra y di la información apropiada para el comando de voz en **Cortana**.

**Requisitos previos:  **

Si acabas de empezar a desarrollar aplicaciones para la Plataforma universal Windows (UWP), consulta estos temas para familiarizarte con las tecnologías presentadas aquí.

-   [Crear tu primera aplicación](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Encontrarás más información acerca de los eventos, en [Introducción a eventos y eventos enrutados](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Directrices sobre la experiencia del usuario:  **

Consulta [Directrices para el diseño de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233) para obtener información sobre cómo integrar tu aplicación con **Cortana** y [Directrices para el diseño de voz](https://msdn.microsoft.com/library/windows/apps/dn596121) para obtener sugerencias prácticas para diseñar una aplicación habilitada para la voz que sea útil y atractiva.

## <span id="Create_a_new_solution_with_a_primary_project_in_Visual_Studio"></span><span id="create_a_new_solution_with_a_primary_project_in_visual_studio"></span><span id="CREATE_A_NEW_SOLUTION_WITH_A_PRIMARY_PROJECT_IN_VISUAL_STUDIO"></span>Crear una nueva solución con un proyecto principal en Visual Studio


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


## <span id="Add_image_assets_to_primary_project_and_specify_them_in_the_app_manifest"></span><span id="add_image_assets_to_primary_project_and_specify_them_in_the_app_manifest"></span><span id="ADD_IMAGE_ASSETS_TO_PRIMARY_PROJECT_AND_SPECIFY_THEM_IN_THE_APP_MANIFEST"></span>Agregar activos de imagen al proyecto principal y especificarlos en el manifiesto de la aplicación
      
Las aplicaciones para UWP pueden seleccionar automáticamente las imágenes más adecuada en función de la configuración específica y las funcionalidades del dispositivo (contraste alto, píxeles efectivos, configuración regional, etc.). Lo único que tienes que hacer es proporcionar las imágenes y asegurarte de que usas la nomenclatura y organización de carpeta apropiadas en el proyecto de aplicación para las diferentes versiones de recursos. Si no proporcionas las versiones de recurso recomendadas, pueden verse afectadas la calidad de imagen, localización y accesibilidad, según las preferencias del usuario, capacidades, tipo de dispositivo y ubicación.

Para más información sobre recursos de imagen para los factores de escala y contraste alto, consulta las [directrices para los activos de icono y el mosaico](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets).

Se asigna nombre a los recursos mediante calificadores. Los calificadores de recursos son modificadores de carpetas y nombres de archivo que identifican el contexto en el que debe usarse una versión particular de un recurso.

La convención de nomenclatura estándar es `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext`. Por ejemplo: `images/en-US/logo.scale-100_contrast-white.png` simplemente se denomina en código mediante la carpeta raíz y el nombre de archivo: `images/logo.png`. Consulta [Cómo asignar un nombre a los recursos mediante calificadores](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324.aspx).

Se recomienda que marques el idioma predeterminado en los archivos de recursos de cadena (como `en-US\resources.resw`) y el factor de escala predeterminado en las imágenes (como `logo.scale-100.png`), incluso si no tienes previstu ofrecer recursos localizados o de resolución múltiple. Sin embargo, como mínimo, te recomendamos que proporciones activos para los factores de escala 100, 200 y 400.

> *Importante

> El icono de la aplicación usado en el área de título del lienzo de **Cortana** es el icono Square44x44Logo especificado en el archivo "Package.appxmanifest". 

> También puedes especificar un icono para cada entrada en el área de contenido del lienzo de **Cortana**. Los tamaños de imagen válidos para los iconos de resultados son:

> - 68 (ancho) x 68 (alto) 
> - 68 (ancho) x 92 (alto) 
> - 280 (ancho) x 140 (alto) 

El icono de contenido no está validado hasta un [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) se pasa a la [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204). Si pasas un objeto **VoiceCommandResponse** a **Cortana** que contiene un icono de contenido con una imagen que no cumple con estas relaciones de tamaño, es posible que se produzca una excepción. 

En este ejemplo de la aplicación **Adventure Works** (VoiceCommandService\\AdventureWorksVoiceCommandService.cs) especificamos un simple cuadrado gris ("GreyTile.png") como el logotipo de la aplicación en [**VoiceCommandContentTile**](https://msdn.microsoft.com/library/windows/apps/dn974168) con la plantilla de ventana [**TitleWith68x68IconAndText**](https://msdn.microsoft.com/library/windows/apps/dn974169). Las variantes de logotipo se encuentran en VoiceCommandService\\Images y se recupera mediante el método [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741).

```CSharp
var destinationTile = new VoiceCommandContentTile();

destinationTile.ContentTileType = 
  VoiceCommandContentTileType.TitleWith68x68IconAndText;
destinationTile.Image = 
  await StorageFile.GetFileFromApplicationUriAsync(
    new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));
```

## <span id="Create_an_app_service_project"></span><span id="create_an_app_service_project"></span><span id="CREATE_AN_APP_SERVICE_PROJECT"></span>Crear un proyecto de servicio de aplicaciones

<ol>
    <li>
    Haz clic con el botón derecho en el nombre de la solución, selecciona **Nuevo > Proyecto**.
    </li>
    <li>
    En **Instalado > Plantillas > Visual C# > Windows > Universal**, selecciona **Componente de Windows Runtime**. Este es el componente que implementa el servicio de aplicaciones (**[Windows.ApplicationModel.AppService](https://msdn.microsoft.com/library/windows/apps/dn921731)**).
    </li>
    <li>
    Escribe un nombre para el proyecto (por ejemplo, "VoiceCommandService") y haz clic en **Aceptar**.
    </li>
    <li>
    En el **Explorador de soluciones**, selecciona el proyecto "VoiceCommandService" y cambia el nombre del archivo "Class1.cs" generado por Visual Studio. Para el ejemplo **Adventure Works**, usamos "AdventureWorksVoiceCommandService.cs".
    </li>
    <li>
    Haz clic en **Sí** cuando te pregunte si quieres cambiar el nombre de todas las apariciones de "Class1.cs". 
    </li>
    <li>
    En el archivo "AdventureWorksVoiceCommandService.cs":
        <ol type="i">
 <li>
 Agrega la siguiente directiva de uso:  
 ```using Windows.ApplicationModel.Background;```
 </li>
 <li>
 Cuando creas un nuevo proyecto, el nombre del proyecto se usa como el espacio de nombres raíz de forma predeterminada en todos los archivos. Cambia el nombre del espacio de nombres para anidar el código de servicio de la aplicación en el proyecto principal. Por ejemplo: `namespace AdventureWorks.VoiceCommands`. 
 </li>
 <li>
 En Explorador de soluciones, haz clic con el botón secundario en el nombre de proyecto del servicio de la aplicación y selecciona **Propiedades**. 
 </li>
 <li>
 En la pestaña **Biblioteca**, actualiza el campo **Espacio de nombres predeterminado** con el mismo valor (para este ejemplo, "AdventureWorks.VoiceCommands"). 
 </li>
 <li>
 Crea una nueva clase que implemente la interfaz [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794). Esta clase requiere un método [**Ejecutar**](https://msdn.microsoft.com/library/windows/apps/br224811) que es el punto de entrada cuando Cortana reconoce el comando de voz. 
 </li>
        </ol>
    </li>
</ol>

Esta es una clase de tarea de segundo plano básica de la aplicación **Adventure Works**. Se completará con más detalle más adelante.
> **Nota**    
> La propia clase de tareas en segundo plano y todas las demás clases en el proyecto de tareas en segundo plano deben ser clases de tipo sealed public.
 
``` csharp
namespace AdventureWorks.VoiceCommands
{
    ...
    
    /// <summary>
    /// The VoiceCommandService implements the entry point for all voice commands.
    /// The individual commands supported are described in the VCD xml file. 
    /// The service entry point is defined in the appxmanifest.
    /// </summary>
    public sealed class AdventureWorksVoiceCommandService : IBackgroundTask
    {
        ...
        
        /// <summary>
        /// The background task entrypoint. 
        /// 
        /// Background tasks must respond to activation by Cortana within 0.5 seconds, and must 
        /// report progress to Cortana every 5 seconds (unless Cortana is waiting for user
        /// input). There is no execution time limit on the background task managed by Cortana,
        /// but developers should use plmdebug (https://msdn.microsoft.com/library/windows/hardware/jj680085%28v=vs.85%29.aspx)
        /// on the Cortana app package in order to prevent Cortana timing out the task during
        /// debugging.
        /// 
        /// The Cortana UI is dismissed if Cortana loses focus. 
        /// The background task is also dismissed even if being debugged. 
        /// Use of Remote Debugging is recommended in order to debug background task behaviors. 
        /// Open the project properties for the app package (not the background task project), 
        /// and enable Debug -> "Do not launch, but debug my code when it starts". 
        /// Alternatively, add a long initial progress screen, and attach to the background task process while it executes.
        /// </summary>
        /// <param name="taskInstance">Connection to the hosting background service process.</param>
        public void Run(IBackgroundTaskInstance taskInstance)
        {
        
          //
          // TODO: Insert code 
          //
          //
        
    }        
  }
}
```

<ol start="7">
    <li>
    Declara la tarea en segundo plano como **AppService** en el manifiesto de la aplicación.
    <ol type="i">
        <li>
        En el **Explorador de soluciones**, haz clic con el botón derecho en el archivo "Package.appxmanifest" y selecciona **Ver código**. 
        </li>
        <li>
        Busca el elemento [**Application**](https://msdn.microsoft.com/library/windows/apps/dn934738).
        </li>
        <li>
        Agregar un elemento [**Extensions**](https://msdn.microsoft.com/library/windows/apps/dn934720) al elemento [**Application**](https://msdn.microsoft.com/library/windows/apps/dn934738).
        </li>
        <li>
        Agrega un elemento [**uap:Extension**](https://msdn.microsoft.com/library/windows/apps/dn986788) al elemento [**Extensions**](https://msdn.microsoft.com/library/windows/apps/dn934720).
        </li>
        <li>Agrega un atributo **Category** al elemento **uap:Extension** y establece el valor del atributo **Category** en "windows.appService".
        </li>
        <li>
        Agrega un atributo **EntryPoint** al elemento **uap:Extension** y establece el valor del atributo **EntryPoint** en el nombre de la clase que implementa [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794), en este caso, "AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService".
        </li>
        <li>
        Agrega un elemento [**uap:AppService**](https://msdn.microsoft.com/library/windows/apps/dn934779) al elemento **uap:Extension**.
        </li>
        <li>
        Agrega un atributo **Name** al elemento [**uap:AppService**](https://msdn.microsoft.com/library/windows/apps/dn934779) y establece el valor del atributo **Name** en un nombre para el servicio de aplicaciones, en este caso, "AdventureWorksVoiceCommandService".
        </li>
        <li>
        Agrega un segundo elemento [**uap:Extension**](https://msdn.microsoft.com/library/windows/apps/dn986788) a [**Extensions**](https://msdn.microsoft.com/library/windows/apps/dn934720).
        </li>
        <li>
        Agrega un atributo **Category** a este elemento [**uap:Extension**](https://msdn.microsoft.com/library/windows/apps/dn986788) y establece el valor del atributo **Category** en "windows.personalAssistantLaunch".
        </li>
    </li> 
    </ol>
    </li>    
</ol>  

Este es el manifiesto de la aplicación Adventure Works:
```xml
<Package>
  <Applications>
    <Application>

      <Extensions>
        <uap:Extension Category="windows.appService" 
          EntryPoint="CortanaBack1.VoiceCommands.CortanaBack1VoiceCommandService">
          <uap:AppService Name="CortanaBack1VoiceCommandService"/>
        </uap:Extension>
        <uap:Extension Category="windows.personalAssistantLaunch"/>
      </Extensions>

    <Application>
  <Applications>
</Package>
```

<ol start="8">
    <li>
    Agrega este proyecto de servicio de la aplicación como referencia en el proyecto principal. 
    <ol type="i">
        <li>
        Haz clic derecho en **Referencias**. 
        </li>
        <li>
        Selecciona **Agregar referencia...**. 
        </li>
        <li>
        En el cuadro de diálogo **Administrador de referencias**, expande **Proyectos** y selecciona el proyecto de servicio de la aplicación. 
        </li>
        <li>
        Haz clic en Aceptar. 
        </li>
    </ol>
    </li>
</ol>

## <span id="Create_a_VCD_file"></span><span id="create_a_vcd_file"></span><span id="CREATE_A_VCD_FILE"></span>Crear un archivo VCD


1. En Visual Studio, haz clic con el botón derecho en el nombre del proyecto principal y selecciona **Agregar > Nuevo elemento**. Agregar un **archivo XML**.
2. Escribe un nombre para el archivo [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) (para este ejemplo, "AdventureWorksCommands.xml") y haz clic en Agregar. 
3. En el **Explorador de soluciones**, selecciona el archivo [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593).
4.  En la ventana **Propiedades**, establece **Acción de compilación** en **Contenido** y luego establece **Copiar en el directorio de salida** en **Copiar si es posterior**.

## <span id="Edit_the_VCD_file"></span><span id="edit_the_vcd_file"></span><span id="EDIT_THE_VCD_FILE"></span>Modificar el archivo VCD

1. Agrega un elemento **VoiceCommands** con un atributo **xmlns** que apunte a `http://schemas.microsoft.com/voicecommands/1.2`.

2. Por cada idioma que admita la aplicación, debes crear un elemento [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) que contenga los comandos de voz que admita la aplicación.

  Se pueden declarar varios elementos [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331), cada uno con un atributo [**XML: lang**](https://msdn.microsoft.com/library/windows/apps/dn722331) atributo para que tu aplicación se use en diferentes mercados. Por ejemplo, una aplicación para los Estados Unidos puede tener un [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) para inglés y otro [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) para español.

  >  **Precaución**  
  Para activar una aplicación e iniciar una acción mediante un comando de voz, la aplicación debe registrar un archivo VCD que contenga un [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) con un idioma que coincida con el idioma de la voz que el usuario seleccionó en su dispositivo. El idioma de voz se encuentra en **Configuración > Sistema > Voz > Idioma de voz**.

3. Agregar un elemento **Command** para cada comando que quieras admitir.

  Cada **Command** declarado en un archivo [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) debe incluir esta información:

  - Un atributo **Name** que la aplicación usa para identificar el comando de voz en tiempo de ejecución. 
  - Un elemento **Example** que contenga una frase que describa cómo puede invocar el comando un usuario. **Cortana** muestra este ejemplo cuando el usuario dice "¿Qué puedo decir?", "Ayuda", o si presiona **Ver más**.    
  -   Un elemento **ListenFor** que contiene las palabras o frases que la aplicación reconoce para iniciar como un comando. Cada elemento **ListenFor** puede contener referencias a uno o más elementos **PhraseList** que contengan palabras específicas apropiadas para el comando.
  > **Nota**  
Los elementos  **ListenFor** no se pueden modificar mediante programación. Sin embargo, los elementos **PhraseList** que están asociados a elementos **ListenFor** se pueden modificar mediante programación. Las aplicaciones deben modificar el contenido del elemento **PhraseList** en tiempo de ejecución, según el conjunto de datos que se genera cuando el usuario usa la aplicación. Consulta [Modificar de forma dinámica listas de frases de definición de comando de voz (VCD)](dynamically-modify-voice-command-definition--vcd--phrase-lists.md).

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
```CSharp
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

## <span id="Handle_activation_and_execute_voice_commands"></span><span id="handle_activation_and_execute_voice_commands"></span><span id="HANDLE_ACTIVATION_AND_EXECUTE_VOICE_COMMANDS"></span>Controlar la activación

Especifica la manera en que tu aplicación responde a activaciones ulteriores de comandos de voz (siempre y cuando se haya iniciado al menos una vez y tengas instalados los conjuntos de comandos de voz).

1.  Comprueba que la aplicación se haya activado mediante un comando de voz.

    Anula el evento [**Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) y comprueba si [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727).[**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) es [**VoiceCommand**](https://msdn.microsoft.com/library/windows/apps/br224693).

2.  Determina el nombre del comando y lo que se ha dicho.

    Obtén una referencia a un objeto [**VoiceCommandActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn609755) de [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727) y consulta la propiedad [**Result**](https://msdn.microsoft.com/library/windows/apps/dn609758) de un objeto [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432).

    Para determinar lo que dijo el usuario, comprueba el valor del elemento [**Text**](https://msdn.microsoft.com/library/windows/apps/dn631441) o de las propiedades semánticas de la frase reconocida en el diccionario [**SpeechRecognitionSemanticInterpretation**](https://msdn.microsoft.com/library/windows/apps/dn631443).

3.  Realiza la acción apropiada en la aplicación como, por ejemplo, ir a la página que quieras.

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

## <span id="Handle_the_voice_command_in_the_app_service"></span><span id="handle_the_voice_command_in_the_app_service"></span><span id="HANDLE_THE_VOICE_COMMAND_IN_THE_APP_SERVICE"></span>Controlar el comando de voz en el servicio de aplicaciones


Procesar el comando de voz en el servicio de aplicaciones.


1.  Agrega las siguientes directivas de uso al archivo de servicio de comandos de voz, "AdventureWorksVoiceCommandService.cs" en este ejemplo.
```csharp
using Windows.ApplicationModel.VoiceCommands;
using Windows.ApplicationModel.Resources.Core;
using Windows.ApplicationModel.AppService;
```

2.  Aplica un aplazamiento de servicio para que el servicio de aplicaciones no finalice mientras se controla el comando de voz.
3.  Confirma que tu tarea en segundo plano se ejecuta como un servicio de aplicaciones que se activó mediante un comando de voz.

    1.  Convierte [**IBackgroundTaskInstance.TriggerDetails**](https://msdn.microsoft.com/library/windows/apps/br224802) a [**Windows.ApplicationModel.AppService.AppServiceTriggerDetails**](https://msdn.microsoft.com/library/windows/apps/dn921727).
    2.  Comprueba que [**IBackgroundTaskInstance.TriggerDetails.Name**](https://msdn.microsoft.com/library/windows/apps/br224807) es el nombre del servicio de aplicaciones del archivo "Package.appxmanifest".

4.  Usa [**IBackgroundTaskInstance.TriggerDetails**](https://msdn.microsoft.com/library/windows/apps/br224802) para crear un [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) en **Cortana** para recuperar el comando de voz.
5.  Registra un controlador de eventos de [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204).[**VoiceCommandCompleted**](https://msdn.microsoft.com/library/windows/apps/dn706584) para recibir notificaciones cuando se cierre el servicio de aplicaciones debido a una cancelación del usuario.
6.  Registra un controlador de eventos de [**IBackgroundTaskInstance.Canceled**](https://msdn.microsoft.com/library/windows/apps/br224798) para recibir notificaciones cuando el servicio de la aplicación se cierre por un error inesperado.
7.  Determina el nombre del comando y lo que se ha dicho.

    1.  Usa la propiedad [**VoiceCommand**](https://msdn.microsoft.com/library/windows/apps/dn974162).[**CommandName**](https://msdn.microsoft.com/library/windows/apps/dn706589) para determinar el nombre del comando de voz.
    2.  Para determinar lo que dijo el usuario, comprueba el valor de [**Text**](https://msdn.microsoft.com/library/windows/apps/dn631441) o las propiedades semánticas de la frase reconocida en el diccionario [**SpeechRecognitionSemanticInterpretation**](https://msdn.microsoft.com/library/windows/apps/dn631443).

7.  Realiza la acción adecuada en el servicio de aplicaciones.
8.  Muestra y di los comentarios al comando de voz con **Cortana**.

    1.  Determina las cadenas que quieres que **Cortana** muestre y diga al usuario en respuesta a los comandos de voz y crea un objeto [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182). Para obtener instrucciones sobre cómo seleccionar las cadenas de comentarios que **Cortana** muestra y dice, consulta las [Directrices de diseño de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233).
    2.  Usa la instancia [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) para informar del progreso o la finalización a **Cortana** con una llamada a [**ReportProgressAsync**](https://msdn.microsoft.com/library/windows/apps/dn706579) o [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) con el objeto **VoiceCommandServiceConnection**.

    Para este ejemplo, volvemos a hacer referencia al archivo VCD del paso 3: Modificar el archivo VCD.

```csharp
public sealed class VoiceCommandService : IBackgroundTask
    {
      private BackgroundTaskDeferral serviceDeferral;
      VoiceCommandServiceConnection voiceServiceConnection;

      public async void Run(IBackgroundTaskInstance taskInstance)
      {
      //Take a service deferral so the service isn&#39;t terminated.
        this.serviceDeferral = taskInstance.GetDeferral();

        taskInstance.Canceled += OnTaskCanceled;

        var triggerDetails = 
          taskInstance.TriggerDetails as AppServiceTriggerDetails;

        if (triggerDetails != null &amp;&amp; 
          triggerDetails.Name == "AdventureWorksVoiceServiceEndpoint")
        {
          try
          {
 voiceServiceConnection = 
   VoiceCommandServiceConnection.FromAppServiceTriggerDetails(
     triggerDetails);

 voiceServiceConnection.VoiceCommandCompleted += 
   VoiceCommandCompleted;

 VoiceCommand voiceCommand = await
 voiceServiceConnection.GetVoiceCommandAsync();

 switch (voiceCommand.CommandName)
 {
   case "whenIsTripToDestination":
   {
     var destination = 
       voiceCommand.Properties["destination"][0];
     SendCompletionMessageForDestination(destination);
     break;
   }

   // As a last resort, launch the app in the foreground.
   default:
     LaunchAppInForeground();
     break;
 }
          }
          finally
          {
 if (this.serviceDeferral != null)
 {
   // Complete the service deferral.
   this.serviceDeferral.Complete();
 }
          }
        }
      }

      private void VoiceCommandCompleted(
        VoiceCommandServiceConnection sender, 
        VoiceCommandCompletedEventArgs args)
      {
        if (this.serviceDeferral != null)
        {
          // Insert your code here.
          // Complete the service deferral.
          this.serviceDeferral.Complete();
        }
      }

      private async void SendCompletionMessageForDestination(
        string destination)
      {
        // Take action and determine when the next trip to destination
        // Insert code here.
        
        // Replace the hardcoded strings used here with strings 
        // appropriate for your application.

        // First, create the VoiceCommandUserMessage with the strings 
        // that Cortana will show and speak.
        var userMessage = new VoiceCommandUserMessage();
        userMessage.DisplayMessage = "Here’s your trip.";
        userMessage.SpokenMessage = "Your trip to Vegas is on August 3rd.";

        // Optionally, present visual information about the answer.
        // For this example, create a VoiceCommandContentTile with an 
        // icon and a string.
        var destinationsContentTiles = new List<VoiceCommandContentTile>();

        var destinationTile = new VoiceCommandContentTile();
        destinationTile.ContentTileType = 
          VoiceCommandContentTileType.TitleWith68x68IconAndText;
        // The user can tap on the visual content to launch the app. 
        // Pass in a launch argument to enable the app to deep link to a 
        // page relevant to the item displayed on the content tile.
        destinationTile.AppLaunchArgument = 
          string.Format("destination={0}”, “Las Vegas");
        destinationTile.Title = "Las Vegas";
        destinationTile.TextLine1 = "August 3rd 2015";
        destinationsContentTiles.Add(destinationTile);

        // Create the VoiceCommandResponse from the userMessage and list    
        // of content tiles.
        var response = 
          VoiceCommandResponse.CreateResponse(
 userMessage, destinationsContentTiles);

        // Cortana will present a “Go to app_name” link that the user 
        // can tap to launch the app. 
        // Pass in a launch to enable the app to deep link to a page 
        // relevant to the voice command.
        response.AppLaunchArgument = 
          string.Format("destination={0}”, “Las Vegas");
        
        // Ask Cortana to display the user message and content tile and 
        // also speak the user message.
        await voiceServiceConnection.ReportSuccessAsync(response);
      }

      private async void LaunchAppInForeground()
      {
        var userMessage = new VoiceCommandUserMessage();
        userMessage.SpokenMessage = "Launching Adventure Works";

        var response = VoiceCommandResponse.CreateResponse(userMessage);

        // When launching the app in the foreground, pass an app 
        // specific launch parameter to indicate what page to show.
        response.AppLaunchArgument = "showAllTrips=true";

        await voiceServiceConnection.RequestAppLaunchAsync(response);
      }
    }
```

Cuando se activa, el servicio de aplicaciones tiene 0,5 segundos para llamar a [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580). **Cortana** usa los datos que proporciona la aplicación para mostrar y decir los comentarios especificados en el archivo VCD. Si la aplicación tarda más de 0,5 segundos en realizar la llamada, **Cortana** inserta una pantalla de entrega, como se muestra aquí. **Cortana** muestra la pantalla de entrega hasta que la aplicación llama a **ReportSuccessAsync**, durante un máximo 5 segundos. Si el servicio de la aplicación no llama a **ReportSuccessAsync** o a alguno de los métodos [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) que proporcionan información a **Cortana**, el usuario recibe un mensaje de error y se cancela el servicio de aplicaciones.

![Una consulta básica con pantallas de progreso y resultado con la aplicación de Adventure Works en segundo plano,](images/cortana-backgroundapp-progress-result.png)

## <span id="related_topics"></span>Artículos relacionados


**Desarrolladores**
* [Interacciones de Cortana](cortana-interactions.md)
* [Definir restricciones de reconocimiento personalizadas](define-custom-recognition-constraints.md)
* [Interactuar con una aplicación en segundo plano en Cortana](interact-with-a-background-app-in-cortana.md)
* [**VCD elements and attributes v1.2 (Elementos y atributos de VCD v1.2)**](https://msdn.microsoft.com/library/windows/apps/dn706593)
* [Inicio rápido: usar recursos de archivo o imagen](https://msdn.microsoft.com/library/windows/apps/xaml/hh965325)
* [Cómo asignar un nombre a los recursos mediante calificadores](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)

**Diseñadores**
* [Directrices para el diseño de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Directrices para el diseño de voz](https://msdn.microsoft.com/library/windows/apps/dn596121)
* [Diseño con capacidad de respuesta 101 para aplicaciones UWP](https://msdn.microsoft.com/library/windows/apps/dn958435)
* [Directrices sobre los activos de icono y de mosaicos](https://msdn.microsoft.com/library/windows/apps/mt412102)

**Muestras**
* [Muestra de comando de voz de Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=May16_HO2-->


