---
description: Usa la API de transferencia en segundo plano para copiar archivos de forma confiable en la red.
title: Transferencias en segundo plano
ms.assetid: 1207B089-BC16-4BF0-BBD4-FD99950C764B
---

# Transferencias en segundo plano

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242)
-   [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998)
-   [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)

Usa la API de transferencia en segundo plano para copiar archivos de forma confiable en la red. La API de transferencia en segundo plano proporciona funciones de carga y descarga avanzadas que se ejecutan en segundo plano durante la suspensión de la aplicación y que persisten tras la finalización de esta. La API supervisa el estado de red, y suspende y reanuda automáticamente las transferencias cuando se pierde la conexión. Además, las transferencias son compatibles con el sensor de datos y el de batería, lo que significa que la actividad de descarga se ajusta según la conectividad y el estado de la batería actuales del dispositivo. La API es ideal para cargar y descargar archivos grandes mediante HTTP(S). También se admite FTP, pero solo para descargas.

La transferencia en segundo plano se ejecuta con independencia de la aplicación que llama y está diseñada principalmente para operaciones de transferencia a largo plazo para recursos como vídeo, música e imágenes de gran tamaño. Para estos escenarios, el uso de transferencias en segundo plano es fundamental, ya que las descargas siguen progresando, incluso aunque se suspenda la aplicación.

Si descargas recursos pequeños que probablemente se completen rápidamente, debes usar las API [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) en vez de una transferencia en segundo plano.

## Uso de Windows.Networking.BackgroundTransfer


### ¿Cómo funciona la característica de transferencia en segundo plano?

Cuando una aplicación usa una transferencia en segundo plano para iniciar una transferencia, la solicitud se configura e inicializa con objetos de clase [**BackgroundDownloader**](https://msdn.microsoft.com/library/windows/apps/br207126) o [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140). El sistema controla cada operación de transferencia por separado y la separa de la aplicación que llama. Dispones de información del progreso si quieres indicar el estado del usuario en la interfaz de usuario de la aplicación. Tu aplicación puede pausar, reanudar, cancelar o incluso leer los datos mientras se realiza la transferencia. La manera en que el sistema controla las transferencias promueve el uso inteligente de energía y evita los problemas que pueden surgir ante eventos que afectan a la aplicación conectada, como la suspensión inesperada de la aplicación, la finalización o cambios repentinos en el estado de red.

### Realización de solicitudes de archivos autenticadas con la transferencia en segundo plano

La transferencia en segundo plano proporciona métodos que admiten credenciales básicas de proxy y servidor, así como el uso de encabezados HTTP personalizados (mediante [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146)) para cada operación de transferencia.

### ¿Cómo se adapta esta característica a los cambios de estado de red o apagados inesperados?

Cuando se producen cambios en el estado de la red, la transferencia en segundo plano mantiene una experiencia coherente para cada operación de transferencia, ya que saca provecho de manera inteligente de la información sobre el estado de la conectividad y el plan de datos del operador, que se proporciona con la característica [Conectividad](https://msdn.microsoft.com/library/windows/apps/hh452990). Para definir el comportamiento en distintos escenarios de red, una aplicación establece una directiva de coste para cada operación usando los valores que [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) define.

Por ejemplo, la directiva de coste definida para una operación puede indicar que se debe pausar automáticamente la operación cuando el dispositivo está usando una red de uso medido. La transferencia se reanudará (o reiniciará) de forma automática cuando se establezca una conexión a una red "sin restricciones". Para obtener más información sobre cómo se definen las redes por coste, consulta [**NetworkCostType**](https://msdn.microsoft.com/library/windows/apps/br207292).

Aunque la característica de transferencia en segundo plano tiene sus propios mecanismos para controlar los cambios de estado de la red, hay otras consideraciones generales sobre conectividad que debes tener en cuenta en relación con las aplicaciones conectadas a la red. Si quieres obtener más información, lee el tema [Aprovechamiento de la información de conexión de red disponible](https://msdn.microsoft.com/library/windows/apps/hh452983).

> **Nota**  Las aplicaciones que se ejecutan en dispositivos móviles tienen características que permiten al usuario supervisar y restringir la cantidad de datos que se transfieren, en función del tipo de conexión, el estado de movilidad y el plan de datos del usuario. Por este motivo, las transferencias en segundo plano se pueden pausar en el teléfono, incluso cuando la [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) indica que la transferencia debe proseguir.

En esta tabla puedes ver cuándo se permiten transferencias en segundo plano en el teléfono para cada valor de [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) según el estado actual del teléfono. Puedes usar la clase [**ConnectionCost**](https://msdn.microsoft.com/library/windows/apps/br207244) para determinar el estado actual del teléfono.

| Estado del dispositivo                                                                                                                      | Solo sin restringir | Predeterminado | Siempre |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------|---------|--------|
| Conectado a Wi-Fi                                                                                                                 | Permitir            | Permitir   | Permitir  |
| Conexión de uso medido, sin roaming, con límite de datos, con seguimiento para permanecer por debajo del límite                                                   | Denegar             | Permitir   | Permitir  |
| Conexión de uso medido, sin roaming, con límite de datos, con seguimiento para superar el límite                                                       | Denegar             | Denegar    | Permitir  |
| Conexión de uso medido, roaming, con límite de datos                                                                                     | Denegar             | Denegar    | Permitir  |
| Conexión de uso medido, por encima del límite de datos. Este estado solo se produce cuando el usuario habilita "Restringir los datos en segundo plano" en la interfaz de usuario de Sensor de datos. | Denegar             | Denegar    | Denegar   |

 

## Carga de archivos


Al usar la transferencia en segundo plano, la carga existe como una [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) que expone varios métodos de control que se usan para reiniciar o cancelar la operación. El sistema controla automáticamente los eventos de la aplicación (por ejemplo, suspensión o finalización) y los cambios en la conectividad por **UploadOperation**; las cargas continuarán durante los periodos de suspensión o pausa de la aplicación y se mantendrán tras la finalización de la misma. Además, al establecer la propiedad [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018), se indica si tu aplicación iniciará las cargas mientras se usa una red de uso medido para la conexión a Internet.

En los siguientes ejemplos, te explicaremos cómo crear e inicializar una carga básica, y cómo enumerar y volver a introducir operaciones persistentes de una sesión anterior de la aplicación.

### Carga de un solo archivo

La creación de una carga comienza con [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140). Esta clase se usa para proporcionar los métodos que permiten a tu aplicación configurar la carga antes de crear la [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) resultante. En el siguiente ejemplo se muestra cómo realizar esto con los objetos [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) y [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) necesarios.

**Identificar el archivo y el destino de la carga**

Antes de comenzar la creación de una [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), debemos identificar el URI de la ubicación en la que se va a realizar la carga y el archivo que se cargará. En el siguiente ejemplo, el valor *uriString* se rellena mediante una cadena de entrada de UI y el valor *file* mediante el objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) devuelto por una operación [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275).

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_B "Identificar el archivo y el destino de la carga")]

**Crear e inicializar la operación de carga**

En el paso anterior, los valores *uriString* y *file* se pasan a una instancia de nuestro siguiente ejemplo, UploadOp, donde se usan para configurar e iniciar la nueva operación de carga. Primero, *uriString* se analiza para crear el objeto [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) necesario.

Después, [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) usa las propiedades del [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) proporcionado (*file*) para rellenar el encabezado de la solicitud y establecer la propiedad *SourceFile* con el objeto **StorageFile**. Después se llama al método [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146) para insertar el nombre de archivo, indicado como una cadena, y la propiedad [**StorageFile.Name**](https://msdn.microsoft.com/library/windows/apps/br227220).

Por último, [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) crea la [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) (*upload*).

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_A "Create and initialize the upload operation")]

Observa las llamadas de método asincrónico definidas con promesas de JavaScript. En una línea del último ejemplo:

```javascript
promise = upload.startAsync().then(complete, error, progress);
```

    The async method call is followed by a then statement which indicates methods, defined by the app, that are called when a result from the async method call is returned. For more information on this programming pattern, see [Asynchronous programming in JavaScript using promises](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx).

### Carga de varios archivos

**Identificar los archivos y el destino de la carga**

    In a scenario involving multiple files transferred with a single [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), the process begins as it usually does by first providing the required destination URI and local file information. Similar to the example in the previous section, the URI is provided as a string by the end-user and [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) can be used to provide the ability to indicate files through the user interface as well. However, in this scenario the app should instead call the [**PickMultipleFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br207851) method to enable the selection of multiple files through the UI.

```javascript
function uploadFiles() {
       var filePicker = new Windows.Storage.Pickers.FileOpenPicker();
       filePicker.fileTypeFilter.replaceAll(["*"]);

       filePicker.pickMultipleFilesAsync().then(function (files) {
          if (files === 0) {
             printLog("No file selected");
                return;
          }

          var upload = new UploadOperation();
          var uriString = document.getElementById("serverAddressField").value;
          upload.startMultipart(uriString, files);

          // Persist the upload operation in the global array.
          uploadOperations.push(upload);
       });
    }
```

**Crear objetos para los parámetros proporcionados**

    The next two examples use code contained in a single example method, **startMultipart**, which was called at the end of the last step. For the purpose of instruction the code in the method that creates an array of [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) objects has been split from the code that creates the resultant [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224).

    First, the URI string provided by the user is initialized as a [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998). Next, the array of [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) objects (**files**) passed to this method is iterated through, each object is used to create a new [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) object which is then placed in the **contentParts** array.

```javascript
upload.startMultipart = function (uriString, files) {
        try {
            var uri = new Windows.Foundation.Uri(uriString);
            var uploader = new Windows.Networking.BackgroundTransfer.BackgroundUploader();

            var contentParts = [];
            files.forEach(function (file, index) {
                var part = new Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart("File" + index, file.name);
                part.setFile(file);
                contentParts.push(part);
            });
```

**Crear e inicializar la operación de carga de varias partes**

    With our contentParts array populated with all of the [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) objects representing each [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) for upload, we are ready to call [**CreateUploadAsync**](https://msdn.microsoft.com/library/windows/apps/hh923973) using the [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) to indicate where the request will be sent.

```javascript
        // Create a new upload operation.
            uploader.createUploadAsync(uri, contentParts).then(function (uploadOperation) {

               // Start the upload and persist the promise to be able to cancel the upload.
               upload = uploadOperation;
               promise = uploadOperation.startAsync().then(complete, error, progress);
            });

         } catch (err) {
             displayError(err);
         }
     };
```

### Reinicio de las operaciones de carga interrumpidas

Después de finalizada o cancelada una [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), se liberan todos los recursos del sistema asociados a ella. Sin embargo, si tu aplicación finaliza antes de que sucedan algunas de estas situaciones, se pausan todas las operaciones activas y se mantienen ocupados los recursos asociados a ellas. Si no se enumeran estas operaciones y se vuelven a introducir en la siguiente sesión de la aplicación, no se completarán y seguirán ocupando recursos del dispositivo.

1.  Antes de definir la función que enumera las operaciones persistentes, necesitamos crear una matriz que contenga los objetos [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) que va a devolver:

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_C "Restart interrupted upload operation")]

2.  Después definimos la función que enumera las operaciones persistentes y las almacena en nuestra matriz. Ten en cuenta que el método **load** al que se llamó para reasignar las devoluciones de llamada a la [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), si es que persiste tras la finalización de la aplicación, se encuentra en la clase UploadOp que definiremos más adelante en esta sección.

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_D "Enumerar operaciones persistentes")]

## Descarga de archivos

Al usar la transferencia en segundo plano, cada descarga existe como una [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) que expone varios métodos de control que se usan para pausar, reanudar, reiniciar o cancelar la operación. El sistema controla automáticamente los eventos de la aplicación (por ejemplo, suspensión o finalización) y los cambios en la conectividad por **DownloadOperation**; las descargas continuarán durante los periodos de suspensión o pausa de la aplicación y se mantendrán tras la finalización de la misma. En escenarios de red móvil, al establecer la propiedad [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018), se indica si tu aplicación iniciará o continuará las descargas mientras se usa una red de uso medido para la conexión a Internet.

Si descargas recursos pequeños que probablemente se completen rápidamente, debes usar las API [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) en vez de una transferencia en segundo plano.

En los siguientes ejemplos, te explicaremos cómo crear e inicializar una descarga básica y cómo enumerar y volver a introducir operaciones persistentes de una sesión anterior de la aplicación.

### Configurar e iniciar una descarga de archivos mediante transferencia en segundo plano

El siguiente ejemplo demuestra cómo las cadenas que representan un URI y un nombre de archivo pueden usarse para crear un objeto [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) y el [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que contendrá el recurso solicitado. En este ejemplo, el nuevo archivo se coloca automáticamente en una ubicación predefinida. Como alternativa, se puede usar [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) para permitir que los usuarios indiquen dónde guardar el archivo en el dispositivo. Ten en cuenta que el método **load** al que se llamó para reasignar las devoluciones de llamada a la [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154), si es que persiste tras la finalización de la aplicación, se encuentra en la clase DownloadOp, que definiremos más adelante en esta sección.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_A)]

Observa las llamadas de método asincrónico definidas con promesas de JavaScript. En la línea 17 del ejemplo de código anterior:

```javascript
promise = download.startAsync().then(complete, error, progress);
```

La llamada de método asincrónico está seguida por una instrucción then que indica métodos definidos por la aplicación a los que se llama cuando se devuelve un resultado de la llamada de método asincrónico. Si quieres obtener más información acerca de este patrón de programación, consulta el tema [Programación asincrónica en JavaScript con promesas](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx).

### Agregar métodos adicionales de control de operaciones

Se puede aumentar el nivel de control implementando métodos [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) adicionales. Por ejemplo, si se agrega el siguiente código al ejemplo anterior, se introduce la capacidad de cancelar la descarga.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_B)]

### Enumeración de operaciones persistentes en el inicio

Después de finalizada o cancelada una [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154), se liberan todos los recursos del sistema asociados a ella. Sin embargo, si tu aplicación finaliza antes de que se produzca alguno de estos eventos, se pausarán las descargas y se mantendrán en segundo plano. Los siguientes ejemplos demuestran cómo volver a introducir descargas persistentes en una nueva sesión de la aplicación.

1.  Antes de definir la función que enumera las operaciones persistentes, necesitamos crear una matriz que contenga los objetos [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) que va a devolver:

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_D)]

2.  Después definimos la función que enumera las operaciones persistentes y las almacena en nuestra matriz. Ten en cuenta que el método **load** al que se llamó para reasignar las devoluciones de llamada para una [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) persistente se encuentra en el ejemplo de DownloadOp que definiremos más adelante en esta sección.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_E)]

3.  Ahora puedes usar la lista rellenada para reiniciar las operaciones pendientes.

## Posprocesamiento

Una nueva característica de Windows 10 es la capacidad de ejecutar código de la aplicación tras la finalización de una transferencia en segundo plano incluso cuando la aplicación no se está ejecutando. Por ejemplo, puede que la aplicación quiera actualizar una lista de películas disponibles una vez finalizada la descarga, en lugar de que la aplicación busque películas nuevas cada vez que se inicie. O puede que la aplicación quiera administrar una transferencia de archivos errónea intentando usar de nuevo un servidor o puerto diferentes. Se invoca un posprocesamiento para transferencias correctas y fallidas, de modo que pueda usarlo para implementar el control de errores y la lógica de reintentos personalizados.

El posprocesamiento usa la infraestructura de tareas en segundo plano existente. Creas una tarea en segundo plano y la asocias con tus transferencias antes de iniciarlas. Después, se ejecutan las transferencias en segundo plano y, una vez completadas, se llama a la tarea en segundo plano para realizar un posprocesamiento.

El posprocesamiento usa una clase nueva, [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209). Esta clase es similar al [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030) existente que permite agrupar transferencias en segundo plano, si bien el **BackgroundTransferCompletionGroup** incluye la posibilidad de designar la ejecución de una tarea en segundo plano cuando se complete la transferencia.

Puedes iniciar una transferencia en segundo plano con posprocesamiento de la siguiente manera.

1.  Crea un objeto [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209). A continuación, crea un objeto [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Establece la propiedad **Trigger** del objeto de generador para el objeto de grupo de finalización y la propiedad **TaskEngtyPoint** del generador para el punto de entrada de la tarea en segundo plano que se debe ejecutar al finalizar la transferencia. Por último, llama al método [**BackgroundTaskBuilder.Register**](https://msdn.microsoft.com/library/windows/apps/br224772) para registrar la tarea en segundo plano. Ten en cuenta que muchos de los grupos de finalización pueden compartir un punto de entrada de tarea en segundo plano, pero solo puedes tener un grupo de finalización por registro de tareas en segundo plano.

   ```csharp
    var completionGroup = new BackgroundTransferCompletionGroup();
    BackgroundTaskBuilder builder = new BackgroundTaskBuilder();

    builder.Name = "MyDownloadProcessingTask";
    builder.SetTrigger(completionGroup.Trigger);
    builder.TaskEntryPoint = "Tasks.BackgroundDownloadProcessingTask";

    BackgroundTaskRegistration downloadProcessingTask = builder.Register();
    ```

2.  Next you associate background transfers with the completion group. Once all transfers are created, enable the completion group.

   ```csharp
    BackgroundDownloader downloader = new BackgroundDownloader(completionGroup);
    DownloadOperation download = downloader.CreateDownload(uri, file);
    Task<DownloadOperation> startTask = download.StartAsync().AsTask();

    // App still sees the normal completion path
    startTask.ContinueWith(ForegroundCompletionHandler);

    // Do not enable the CompletionGroup until after all downloads are created.
    downloader.CompletinGroup.Enable();
    ```

3.  The code in the background task extracts the list of operations from the trigger details, and your code can then inspect the details for each operation and perform appropriate post-processing for each operation.

   ```csharp
    public class BackgroundDownloadProcessingTask : IBackgroundTask
    {
      public async void Run(IBackgroundTaskInstance taskInstance)
      {
        var details = (BackgroundTransferCompletionGroupTriggerDetails)taskInstance.TriggerDetails;
        IReadOnlyList<DownloadOperation> downloads = details.Downloads;

        // Do post-processing on each finished operation in the list of downloads
      }
    }
    ```

The post-processing task is a regular background task. It is part of the pool of all background tasks, and it is subject to the same resource management policy as all background tasks.

Also, note that post-processing does not replace foreground completion handlers. If your app defines a foreground completion handler, and your app is running when the file transfer completes, then both your foreground completion handler and your background completion handler will be called. The order in which foreground and background tasks are called is not guaranteed. If you define both, you should ensure that the two tasks will work properly and not interfere with each other if they are running concurrently.

## Request timeouts

There are two primary connection timeout scenarios to take into consideration:

-   When establishing a new connection for a transfer, the connection request is aborted if it is not established within five minutes.

-   After a connection has been established, an HTTP request message that has not received a response within two minutes is aborted.

> **Note**  In either scenario, assuming there is Internet connectivity, Background Transfer will retry a request up to three times automatically. In the event Internet connectivity is not detected, additional requests will wait until it is.

## Debugging guidance

Stopping a debugging session in Microsoft Visual Studio is comparable to closing your app; PUT uploads are paused and POST uploads are terminated. Even while debugging, your app should enumerate and then restart or cancel any persisted uploads. For example, you can have your app cancel enumerated persisted upload operations at app startup if there is no interest in previous operations for that debug session.

While enumerating downloads/uploads on app startup during a debug session, you can have your app cancel them if there is no interest in previous operations for that debug session. Note that if there are Visual Studio project updates, like changes to the app manifest, and the app is uninstalled and re-deployed, [**GetCurrentUploadsAsync**](https://msdn.microsoft.com/library/windows/apps/hh701149) cannot enumerate operations created using the previous app deployment.

When using Background Transfer during development, you may get into a situation where the internal caches of active and completed transfer operations can get out of sync. This may result in the inability to start new transfer operations or interact with existing operations and [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030) objects. In some cases, attempting to interact with existing operations may trigger a crash. This result can occur if the [**TransferBehavior**](https://msdn.microsoft.com/library/windows/apps/dn279033) property is set to **Parallel**. This issue occurs only in certain scenarios during development and is not applicable to end users of your app.

Four scenarios using Visual Studio can cause this issue.

-   You create a new project with the same app name as an existing project, but a different language (from C++ to C#, for example).
-   You change the target architecture (from x86 to x64, for example) in an existing project.
-   You change the culture (from neutral to en-US, for example) in an existing project.
-   You add or remove a capability in the package manifest (adding **Enterprise Authentication**, for example) in an existing project.

Regular app servicing, including manifest updates which add or remove capabilities, do not trigger this issue on end user deployments of your app.
To work around this issue, completely uninstall all versions of the app and re-deploy with the new language, architecture, culture, or capability. This can be done via the **Start** screen or using PowerShell and the **Remove-AppxPackage** cmdlet.

## Exceptions in Windows.Networking.BackgroundTransfer

An exception is thrown when an invalid string for a the Uniform Resource Identifier (URI) is passed to the constructor for the [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) object.

**.NET:  **The [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) type appears as [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) in C# and VB.

In C# and Visual Basic, this error can be avoided by using the [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) class in the .NET 4.5 and one of the [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) methods to test the string received from the app user before the URI is constructed.

In C++, there is no method to try and parse a string to a URI. If an app gets input from the user for the [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998), the constructor should be in a try/catch block. If an exception is thrown, the app can notify the user and request a new hostname.

The [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) namespace has convenient helper methods and uses enumerations in the [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) namespace for handling errors. This can be useful for handling specific network exceptions differently in your app.

An error encountered on an asynchronous method in the [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) namespace is returned as an **HRESULT** value. The [**BackgroundTransferError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701093) method is used to convert a network error from a background transfer operation to a [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818) enumeration value. Most of the **WebErrorStatus** enumeration values correspond to an error returned by the native HTTP or FTP client operation. An app can filter on specific **WebErrorStatus** enumeration values to modify app behavior depending on the cause of the exception.

For parameter validation errors, an app can also use the **HRESULT** from the exception to learn more detailed information on the error that caused the exception. Possible **HRESULT** values are listed in the *Winerror.h* header file. For most parameter validation errors, the **HRESULT** returned is **E\_INVALIDARG**.



<!--HONumber=Mar16_HO1-->


