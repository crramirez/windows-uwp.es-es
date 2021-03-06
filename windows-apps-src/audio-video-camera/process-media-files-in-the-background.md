---
ms.assetid: B5E3A66D-0453-4D95-A3DB-8E650540A300
description: En este artículo se muestra cómo usar la clase MediaProcessingTrigger y una tarea en segundo plano para procesar los archivos multimedia en segundo plano.
title: Procesar archivos multimedia en segundo plano
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b7f726365a2e476e650f4b66d484840c0efabda5
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363828"
---
# <a name="process-media-files-in-the-background"></a>Procesar archivos multimedia en segundo plano



En este artículo se muestra cómo usar [**MediaProcessingTrigger**](/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTrigger) y una tarea en segundo plano para procesar archivos multimedia en segundo plano.

La aplicación de ejemplo descrita en este artículo permite al usuario seleccionar un archivo multimedia de entrada para transcodificar y especificar un archivo de salida para el resultado de la transcodificación. A continuación, se inicia una tarea en segundo plano para realizar la operación de transcodificación. La clase [**MediaProcessingTrigger**](/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTrigger) se usa para admitir abundantes y diversos escenarios de procesamiento de medios entre los que se incluyen, además de la transcodificación, la representación de composiciones multimedia en el disco y la carga de archivos multimedia procesados una vez finalizado el procesamiento.

Para obtener información más detallada sobre las diferentes características de la aplicación universal de Windows que se usa en esta muestra, consulta:

-   [Transcodificar archivos multimedia](transcode-media-files.md)
-   [Iniciar reanudación y tareas en segundo plano](../launch-resume/index.md)
-   [Distintivos y notificaciones de iconos](../design/shell/tiles-and-notifications/index.md)

## <a name="create-a-media-processing-background-task"></a>Crear una tarea en segundo plano de procesamiento de multimedia

Para agregar una tarea en segundo plano a la solución existente en Microsoft Visual Studio, escribe un nombre para la composición.

1.  En el menú **archivo** , seleccione **Agregar** y, a continuación, **nuevo proyecto.**...
2.  Seleccione el tipo de proyecto **Windows Runtime componente (Windows universal)**.
3.  Escribe un nombre para el nuevo proyecto de componente. En este ejemplo se usa el nombre de proyecto **MediaProcessingBackgroundTask**.
4.  Haga clic en Aceptar.

En el **Explorador de soluciones**, haz clic con el botón secundario en el icono del archivo "Class1.cs" que se crea de forma predeterminada, y selecciona **Cambiar nombre**. Cambia el nombre del archivo a "MediaProcessingTask.cs". Cuando Visual Studio le pregunte si desea cambiar el nombre de todas las referencias a esta clase, haga clic en **sí**.

En el archivo de clase que ha cambiado de nombre, agrega las siguientes directivas de tipo **using** para incluir estos espacios de nombres en el proyecto.
                                  
:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs" id="SnippetBackgroundUsing":::

Actualice su declaración de clase para que la clase herede de [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs" id="SnippetBackgroundClass":::

Agrega las siguientes variables de miembro a tu clase:

-   Una interfaz [**IBackgroundTaskInstance**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) que se usará para actualizar la aplicación en primer plano mediante el progreso de la tarea en segundo plano.
-   Una clase [**BackgroundTaskDeferral**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskDeferral) que evitará que el sistema cierre la tarea en segundo plano mientras se realiza la transcodificación de archivos multimedia de forma asincrónica.
-   Un objeto **CancellationTokenSource** que se puede usar para cancelar la operación de transcodificación asincrónica.
-   El objeto [**MediaTranscoder**](/uwp/api/Windows.Media.Transcoding.MediaTranscoder), que se usará para transcodificar archivos multimedia.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs" id="SnippetBackgroundMembers":::

Una vez hecho esto, el sistema llamará al método [**Run**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) de una tarea en segundo plano cuando se inicie la tarea. Debes establecer el objeto [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) que se pasó al método de la variable de miembro correspondiente. A continuación, registra un controlador para el evento [**Canceled**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.canceled), que se producirá si el sistema lo necesita para cerrar la tarea en segundo plano. Seguidamente, establece la propiedad [**Progress**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.progress) en cero.

Deberás llamar al método [**GetDeferral**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.getdeferral) del objeto de la tarea en segundo plano para obtener un aplazamiento. Esta acción le dice al sistema que no cierre la tarea porque estás realizando operaciones asincrónicas.

El siguiente paso es llamar al método auxiliar **TranscodeFileAsync**, que se define en la siguiente sección. Si esto se completa correctamente, se llama a un método auxiliar para que inicie una notificación del sistema que avisará al usuario de que se ha completado la transcodificación.

Al final del método **Run**, llama al método [**Complete**](/uwp/api/windows.applicationmodel.background.backgroundtaskdeferral.complete) del objeto de aplazamiento para que el sistema sepa que la tarea en segundo plano se ha completado y que se puede finalizar.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs" id="SnippetRun":::

En el método auxiliar **TranscodeFileAsync**, los nombres de los archivos de entrada y salida de las operaciones de transcodificación se recuperan de la propiedad [**LocalSettings**](/uwp/api/windows.storage.applicationdata.localsettings) de la aplicación. La aplicación en primer plano se encargará de establecer estos valores. Crea un objeto [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) para los archivos de entrada y salida y, a continuación, crea un perfil de codificación para usar en la transcodificación.

Llama a [**PrepareFileTranscodeAsync**](/uwp/api/windows.media.transcoding.mediatranscoder.preparefiletranscodeasync) y pasa el archivo de entrada, el archivo de salida y el perfil de codificación. Recuerda que el objeto [**PrepareTranscodeResult**](/uwp/api/Windows.Media.Transcoding.PrepareTranscodeResult) que devuelve esta llamada te permite saber si se puede realizar la transcodificación. Si resulta que la propiedad [**CanTranscode**](/uwp/api/windows.media.transcoding.preparetranscoderesult.cantranscode) es "true", llama a [**TranscodeAsync**](/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) para realizar la operación de transcodificación.

El método **AsTask** te permite realizar un seguimiento del progreso de la operación asincrónica o cancelarla. Puedes crear un objeto **Progress** y especificar las unidades de progreso que quieras y el nombre del método al que se llamará para notificarte el progreso actual de la tarea. A continuación, pasa el objeto **Progress** al método **AsTask** junto con el token de cancelación que te permitirá cancelar la tarea.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs" id="SnippetTranscodeFileAsync":::

En el método que usaste para crear el objeto **Progress** en el paso anterior, establece el progreso de la instancia de la tarea en segundo plano. Esta acción te permitirá pasar el progreso a la aplicación en primer plano, si se está ejecutando.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs" id="SnippetProgress":::

El método auxiliar **SendToastNotification** crea una nueva notificación del sistema mediante la obtención de un documento XML de plantilla de una notificación del sistema que solo tiene contenido de texto. De esta manera se establecerá el elemento de texto de la notificación del sistema XML y se creará un nuevo objeto [**ToastNotification**](/uwp/api/Windows.UI.Notifications.ToastNotification) a partir del documento XML. Por último, puedes mostrar al usuario la notificación del sistema mediante una llamada a [**ToastNotifier.Show**](/uwp/api/windows.ui.notifications.toastnotifier.show).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs" id="SnippetSendToastNotification":::

En el controlador para el evento [**Canceled**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.canceled), que se llama cuando el sistema cancela la tarea en segundo plano, puedes registrar el error con fines de telemetría.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs" id="SnippetOnCanceled":::

## <a name="register-and-launch-the-background-task"></a>Registrar y cargar la tarea en segundo plano

Para poder iniciar la tarea en segundo plano desde la aplicación en primer plano, debes actualizar el archivo Package.appmanifest de la aplicación en primer plano para que el sistema sepa que esta aplicación usa una tarea en segundo plano.

1.  En el **Explorador de soluciones**, haz doble clic en el icono del archivo Package.appmanifest para abrir el editor del manifiesto.
2.  Selecciona la pestaña **Declaraciones**.
3.  En **Declaraciones disponibles**, selecciona **Tareas en segundo plano** y haz clic en **Agregar**.
4.  En **Declaraciones admitidas**, asegúrate de que esté seleccionado el elemento **Tareas en segundo plano**. En **Propiedades**, selecciona la casilla de verificación **Procesamiento multimedia**.
5.  En el cuadro de texto **Punto de entrada**, especifica el espacio de nombres y el nombre de clase para la prueba en segundo plano, separados por un punto. Para este ejemplo, la entrada es:
   ```csharp
   MediaProcessingBackgroundTask.MediaProcessingTask
   ```
A continuación, debes agregar una referencia a la tarea en segundo plano a tu aplicación en primer plano.
1.  En **Explorador de soluciones**, en el proyecto de la aplicación de primer plano, haga clic con el botón derecho en la carpeta **referencias** y seleccione **Agregar referencia..**..
2.  Expande el nodo **Proyectos** y selecciona **Solución**.
3.  Active la casilla situada junto a su proyecto de tarea en segundo plano y haga clic en **Aceptar**.

El resto del código de este ejemplo debe agregarse a la aplicación en primer plano. En primer lugar, debes agregar los siguientes espacios de nombres al proyecto.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs" id="SnippetForegroundUsing":::

A continuación, agrega las siguientes variables de miembro que son necesarias para registrar la tarea en segundo plano.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs" id="SnippetForegroundMembers":::

El método auxiliar **PickFilesToTranscode** usa las clases [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) y [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) para abrir los archivos de entrada y salida de la transcodificación. Gracias a esto, el usuario puede seleccionar archivos en una ubicación a la que tu aplicación no tiene acceso. Para asegurarte de que tu tarea en segundo plano pueda abrir los archivos, agrégalos a la propiedad [**FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) de la aplicación.

Finalmente, establece los nombres de archivo de entrada y de salida en la propiedad [**LocalSettings**](/uwp/api/windows.storage.applicationdata.localsettings) de la aplicación. La tarea en segundo plano recupera los nombres de archivo de esta ubicación.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs" id="SnippetPickFilesToTranscode":::

Para registrar la tarea en segundo plano, crea las clases [**MediaProcessingTrigger**](/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTrigger) y [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder). A continuación, establece el nombre del generador de la tarea en segundo plano para poder identificarla más tarde. Establece la propiedad [**TaskEntryPoint**](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.taskentrypoint) en la misma cadena de nombres de clase y espacio de nombres que usaste en el archivo de manifiesto. Una vez hecho esto, establece la propiedad [**Trigger**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.trigger) en la instancia **MediaProcessingTrigger**.

Antes de registrar la tarea, asegúrate de anular el registro de cualquier tarea registrada previamente; para ello, revisa la colección [**AllTasks**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks) y llama al método [**Unregister**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskregistration.unregister) de las tareas que tengan el nombre que especificaste en la propiedad [**BackgroundTaskBuilder.Name**](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.name).

Registra la tarea en segundo plano llamando al método [**Register**](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register). Seguidamente, registra los controladores de los eventos [**Completed**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.completed) y [**Progress**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskregistration.progress).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs" id="SnippetRegisterBackgroundTask":::

Una aplicación típica registrará su tarea en segundo plano cuando la aplicación se inicie inicialmente, como en el evento **OnNavigatedTo** .

Inicia la tarea en segundo plano llamando al método [**RequestAsync**](/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger.requestasync) del objeto **MediaProcessingTrigger**. El objeto [**MediaProcessingTriggerResult**](/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTriggerResult) que devuelve este método te permitirá saber si la tarea en segundo plano se inició correctamente y, si no es así, te permitirá saber el por qué. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs" id="SnippetLaunchBackgroundTask":::

Una aplicación típica iniciará la tarea en segundo plano en respuesta a la interacción del usuario, como en el evento de **clic** de un control de IU.

Se llama al controlador de eventos **OnProgress** cuando la tarea en segundo plano actualiza el progreso de la operación. Puedes usar esta oportunidad para actualizar la interfaz de usuario con la información de progreso.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs" id="SnippetOnProgress":::

Se llama al controlador de eventos **OnCompleted** cuando la tarea en segundo plano termina de ejecutarse. Esta es otra oportunidad para actualizar la interfaz de usuario y proporcionar información de estado al usuario.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs" id="SnippetOnCompleted":::


 

 
