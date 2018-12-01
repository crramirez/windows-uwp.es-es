---
ms.assetid: B5E3A66D-0453-4D95-A3DB-8E650540A300
description: En este artículo se muestra cómo usar la clase MediaProcessingTrigger y una tarea en segundo plano para procesar los archivos multimedia en segundo plano.
title: Procesar archivos multimedia en segundo plano
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0194ccba43e2ba5270b9ff8eacf045ca140af6cb
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8338284"
---
# <a name="process-media-files-in-the-background"></a>Procesar archivos multimedia en segundo plano



En este artículo se muestra cómo usar la clase [**MediaProcessingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806005) y una tarea en segundo plano para procesar los archivos multimedia en segundo plano.

La aplicación de ejemplo descrita en este artículo permite al usuario seleccionar un archivo multimedia de entrada para transcodificar y especificar un archivo de salida para el resultado de la transcodificación. A continuación, se inicia una tarea en segundo plano para realizar la operación de transcodificación. La clase [**MediaProcessingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806005) se usa para admitir abundantes y diversos escenarios de procesamiento de medios entre los que se incluyen, además de la transcodificación, la representación de composiciones multimedia en el disco y la carga de archivos multimedia procesados una vez finalizado el procesamiento.

Para obtener información más detallada sobre las diferentes características de la aplicación universal de Windows que se usa en esta muestra, consulta:

-   [Transcodificar archivos multimedia](transcode-media-files.md)
-   [Inicio, reanudación y tareas en segundo plano](https://msdn.microsoft.com/library/windows/apps/mt227652)
-   [Distintivos de mosaico y notificaciones](https://msdn.microsoft.com/library/windows/apps/mt185606)

## <a name="create-a-media-processing-background-task"></a>Crear una tarea en segundo plano de procesamiento de multimedia

Para agregar una tarea en segundo plano a la solución existente en Microsoft Visual Studio, escribe un nombre para la composición.

1.  En el menú **Archivo**, selecciona **Agregar** y luego **Nuevo proyecto...**.
2.  Selecciona el tipo de proyecto **Componente de Windows Runtime (Windows Universal)**.
3.  Escribe un nombre para el nuevo proyecto de componente. En este ejemplo usaremos el nombre de proyecto **MediaProcessingBackgroundTask**.
4.  Haz clic en Aceptar.

En el **Explorador de soluciones**, haz clic con el botón derecho en el icono del archivo "Class1.cs", que se crea de forma predeterminada, y selecciona **Cambiar nombre**. Cambia el nombre del archivo a "MediaProcessingTask.cs". Cuando Visual Studio te pregunte si deseas cambiar el nombre de todas las referencias a esta clase, haz clic en **Sí**.

En el archivo de clase que ha cambiado de nombre, agrega las siguientes directivas de tipo **using** para incluir estos espacios de nombres en el proyecto.
                                  
[!code-cs[BackgroundUsing](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundUsing)]

Actualiza la declaración de clase para que su clase herede de [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794).

[!code-cs[BackgroundClass](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundClass)]

Agrega las siguientes variables de miembro a tu clase:

-   Una interfaz [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797) que se usará para actualizar la aplicación en primer plano mediante el progreso de la tarea en segundo plano.
-   Una clase [**BackgroundTaskDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700499) que evitará que el sistema cierre la tarea en segundo plano mientras se realiza la transcodificación de archivos multimedia de forma asincrónica.
-   Un objeto **CancellationTokenSource** que se puede usar para cancelar la operación de transcodificación asincrónica.
-   El objeto [**MediaTranscoder**](https://msdn.microsoft.com/library/windows/apps/br207080), que se usará para transcodificar archivos multimedia.

[!code-cs[BackgroundMembers](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundMembers)]

Una vez hecho esto, el sistema llamará al método [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) de una tarea en segundo plano cuando se inicie la tarea. Debes establecer el objeto [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) que se pasó al método de la variable de miembro correspondiente. A continuación, registra un controlador para el evento [**Canceled**](https://msdn.microsoft.com/library/windows/apps/br224798), que se producirá si el sistema lo necesita para cerrar la tarea en segundo plano. Seguidamente, establece la propiedad [**Progress**](https://msdn.microsoft.com/library/windows/apps/br224800) en cero.

Deberás llamar al método [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700507) del objeto de la tarea en segundo plano para obtener un aplazamiento. Esta acción le dice al sistema que no cierre la tarea porque estás realizando operaciones asincrónicas.

El siguiente paso es llamar al método auxiliar **TranscodeFileAsync**, que se define en la siguiente sección. Si esto se completa correctamente, se llama a un método auxiliar para que inicie una notificación del sistema que avisará al usuario de que se ha completado la transcodificación.

Al final del método **Run**, llama al método [**Complete**](https://msdn.microsoft.com/library/windows/apps/hh700504) del objeto de aplazamiento para que el sistema sepa que la tarea en segundo plano se ha completado y que se puede finalizar.

[!code-cs[Run](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetRun)]

En el método auxiliar **TranscodeFileAsync**, los nombres de los archivos de entrada y salida de las operaciones de transcodificación se recuperan de la propiedad [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) de la aplicación. La aplicación en primer plano se encargará de establecer estos valores. Crea un objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) para los archivos de entrada y salida y, a continuación, crea un perfil de codificación para usar en la transcodificación.

Llama a [**PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936) y pasa el archivo de entrada, el archivo de salida y el perfil de codificación. Recuerda que el objeto [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941) que devuelve esta llamada te permite saber si se puede realizar la transcodificación. Si resulta que la propiedad [**CanTranscode**](https://msdn.microsoft.com/library/windows/apps/hh700942) es "true", llama a [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) para realizar la operación de transcodificación.

El método **AsTask** te permite realizar un seguimiento del progreso de la operación asincrónica o cancelarla. Puedes crear un objeto **Progress** y especificar las unidades de progreso que quieras y el nombre del método al que se llamará para notificarte el progreso actual de la tarea. A continuación, pasa el objeto **Progress** al método **AsTask** junto con el token de cancelación que te permitirá cancelar la tarea.

[!code-cs[TranscodeFileAsync](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetTranscodeFileAsync)]

En el método que usaste para crear el objeto **Progress** en el paso anterior, establece el progreso de la instancia de la tarea en segundo plano. Esta acción te permitirá pasar el progreso a la aplicación en primer plano, si se está ejecutando.

[!code-cs[Progress](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetProgress)]

El método auxiliar **SendToastNotification** crea una nueva notificación del sistema mediante la obtención de un documento XML de plantilla de una notificación del sistema que solo tiene contenido de texto. De esta manera se establecerá el elemento de texto de la notificación del sistema XML y se creará un nuevo objeto [**ToastNotification**](https://msdn.microsoft.com/library/windows/apps/br208641) a partir del documento XML. Por último, puedes mostrar al usuario la notificación del sistema mediante una llamada a [**ToastNotifier.Show**](https://msdn.microsoft.com/library/windows/apps/br208659).

[!code-cs[SendToastNotification](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetSendToastNotification)]

En el controlador para el evento [**Canceled**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.IBackgroundTaskInstance.Canceled), que se llama cuando el sistema cancela la tarea en segundo plano, puedes registrar el error con fines de telemetría.

[!code-cs[OnCanceled](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetOnCanceled)]

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
1.  En el **Explorador de soluciones**, en el proyecto de la aplicación en primer plano, haz clic con el botón secundario en la carpeta **Referencias** y selecciona **Agregar referencia...**.
2.  Expande el nodo **Proyectos** y selecciona **Solución**.
3.  Activa la casilla situada junto a tu proyecto de tarea en segundo plano y haz clic en **Aceptar**.

El resto del código de este ejemplo debe agregarse a la aplicación en primer plano. En primer lugar, debes agregar los siguientes espacios de nombres al proyecto.

[!code-cs[ForegroundUsing](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetForegroundUsing)]

A continuación, agrega las siguientes variables de miembro que son necesarias para registrar la tarea en segundo plano.

[!code-cs[ForegroundMembers](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetForegroundMembers)]

El método auxiliar **PickFilesToTranscode** usa las clases [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) y [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) para abrir los archivos de entrada y salida de la transcodificación. Gracias a esto, el usuario puede seleccionar archivos en una ubicación a la que tu aplicación no tiene acceso. Para asegurarte de que tu tarea en segundo plano pueda abrir los archivos, agrégalos a la propiedad [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) de la aplicación.

Finalmente, establece los nombres de archivo de entrada y de salida en la propiedad [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) de la aplicación. La tarea en segundo plano recupera los nombres de archivo de esta ubicación.

[!code-cs[PickFilesToTranscode](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetPickFilesToTranscode)]

Para registrar la tarea en segundo plano, crea las clases [**MediaProcessingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806005) y [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). A continuación, establece el nombre del generador de la tarea en segundo plano para poder identificarla más tarde. Establece la propiedad [**TaskEntryPoint**](https://msdn.microsoft.com/library/windows/apps/br224774) en la misma cadena de nombres de clase y espacio de nombres que usaste en el archivo de manifiesto. Una vez hecho esto, establece la propiedad [**Trigger**](https://msdn.microsoft.com/library/windows/apps/dn641725) en la instancia **MediaProcessingTrigger**.

Antes de registrar la tarea, asegúrate de anular el registro de cualquier tarea registrada previamente; para ello, revisa la colección [**AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787) y llama al método [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229870) de las tareas que tengan el nombre que especificaste en la propiedad [**BackgroundTaskBuilder.Name**](https://msdn.microsoft.com/library/windows/apps/br224771).

Registra la tarea en segundo plano llamando al método [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772). Seguidamente, registra los controladores de los eventos [**Completed**](https://msdn.microsoft.com/library/windows/apps/br224788) y [**Progress**](https://msdn.microsoft.com/library/windows/apps/br224808).

[!code-cs[RegisterBackgroundTask](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetRegisterBackgroundTask)]

Una aplicación típica registra su tarea en segundo plano cuando la aplicación iniciada inicialmente, como en el evento **OnNavigatedTo** .

Inicia la tarea en segundo plano llamando al método [**RequestAsync**](https://msdn.microsoft.com/library/windows/apps/dn765071) del objeto **MediaProcessingTrigger**. El objeto [**MediaProcessingTriggerResult**](https://msdn.microsoft.com/library/windows/apps/dn806007) que devuelve este método te permitirá saber si la tarea en segundo plano se inició correctamente y, si no es así, te permitirá saber el por qué. 

[!code-cs[LaunchBackgroundTask](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetLaunchBackgroundTask)]

Una aplicación típica iniciará la tarea en segundo plano en respuesta a la interacción del usuario, como en el evento **Click** de un control de la interfaz de usuario.

Se llama al controlador de eventos **OnProgress** cuando la tarea en segundo plano actualiza el progreso de la operación. Puedes usar esta oportunidad para actualizar la interfaz de usuario con la información de progreso.

[!code-cs[OnProgress](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetOnProgress)]

Se llama al controlador de eventos **OnCompleted** cuando la tarea en segundo plano termina de ejecutarse. Esta es otra oportunidad para actualizar la interfaz de usuario y proporcionar información de estado al usuario.

[!code-cs[OnCompleted](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetOnCompleted)]


 

 




