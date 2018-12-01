---
description: Usa la API de transferencia en segundo plano para copiar archivos de forma confiable en la red.
title: Transferencias en segundo plano
ms.assetid: 1207B089-BC16-4BF0-BBD4-FD99950C764B
ms.date: 03/23/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 3d67bb7c7fd2173e1406669367935efdb09967ea
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2018
ms.locfileid: "8351775"
---
# <a name="background-transfers"></a>Transferencias en segundo plano
Usa la API de transferencia en segundo plano para copiar archivos de forma confiable en la red. La API de transferencia en segundo plano proporciona funciones de carga y descarga avanzadas que se ejecutan en segundo plano durante la suspensión de la aplicación y que persisten tras la finalización de esta. La API supervisa el estado de red, y suspende y reanuda automáticamente las transferencias cuando se pierde la conexión. Además, las transferencias son compatibles con el sensor de datos y el de batería, lo que significa que la actividad de descarga se ajusta según la conectividad y el estado de la batería actuales del dispositivo. La API es ideal para cargar y descargar archivos grandes mediante HTTP(S). También se admite FTP, pero solo para descargas.

La transferencia en segundo plano se ejecuta con independencia de la aplicación que llama y está diseñada principalmente para operaciones de transferencia a largo plazo para recursos como vídeo, música e imágenes de gran tamaño. Para estos escenarios, el uso de transferencias en segundo plano es fundamental, ya que las descargas siguen progresando, incluso aunque se suspenda la aplicación.

Si descargas recursos pequeños que probablemente se completen rápidamente, debes usar las API [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) en vez de una transferencia en segundo plano.

## <a name="using-windowsnetworkingbackgroundtransfer"></a>Uso de Windows.Networking.BackgroundTransfer

### <a name="how-does-the-background-transfer-feature-work"></a>¿Cómo funciona la característica de transferencia en segundo plano?
Cuando una aplicación usa una transferencia en segundo plano para iniciar una transferencia, la solicitud se configura e inicializa con objetos de clase [**BackgroundDownloader**](https://msdn.microsoft.com/library/windows/apps/br207126) o [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140). El sistema controla cada operación de transferencia por separado y la separa de la aplicación que llama. Dispones de información del progreso si quieres indicar el estado del usuario en la interfaz de usuario de la aplicación. Tu aplicación puede pausar, reanudar, cancelar o incluso leer los datos mientras se realiza la transferencia. La manera en que el sistema controla las transferencias promueve el uso inteligente de energía y evita los problemas que pueden surgir ante eventos que afectan a la aplicación conectada, como la suspensión inesperada de la aplicación, la finalización o cambios repentinos en el estado de red.

> [!NOTE]
> Debido a restricciones de recursos por aplicación, una aplicación no debe tener más de 200 transferencias (DownloadOperations + UploadOperations) en cualquier momento. La superación de ese límite puede dejar la cola de transferencia de la aplicación en un estado irrecuperable.

Cuando se inicia una aplicación, debe llamar [**AttachAsync**](/uwp/api/windows.networking.backgroundtransfer.downloadoperation.AttachAsync) en todos los objetos de [**DownloadOperation**](/uwp/api/windows.networking.backgroundtransfer.downloadoperation?branch=live) y [**UploadOperation**](/uwp/api/windows.networking.backgroundtransfer.uploadperation?branch=live) existentes. No hacer esto hará que la pérdida de las transferencias ya completado y finalmente representará el uso de la característica de transferencia en segundo plano sea inútil.

### <a name="performing-authenticated-file-requests-with-background-transfer"></a>Realización de solicitudes de archivos autenticadas con la transferencia en segundo plano
La transferencia en segundo plano proporciona métodos que admiten credenciales básicas de proxy y servidor, así como el uso de encabezados HTTP personalizados (mediante [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146)) para cada operación de transferencia.

### <a name="how-does-this-feature-adapt-to-network-status-changes-or-unexpected-shutdowns"></a>¿Cómo se adapta esta característica a los cambios de estado de red o apagados inesperados?
Cuando se producen cambios en el estado de la red, la transferencia en segundo plano mantiene una experiencia coherente para cada operación de transferencia, ya que saca provecho de manera inteligente de la información sobre el estado de la conectividad y el plan de datos del operador, que se proporciona con la característica [Conectividad](https://msdn.microsoft.com/library/windows/apps/hh452990). Para definir el comportamiento en distintos escenarios de red, una aplicación establece una directiva de coste para cada operación usando los valores que [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) define.

Por ejemplo, la directiva de coste definida para una operación puede indicar que se debe pausar automáticamente la operación cuando el dispositivo está usando una red de uso medido. La transferencia se reanudará (o reiniciará) de forma automática cuando se establezca una conexión a una red "sin restricciones". Para obtener más información sobre cómo se definen las redes por coste, consulta [**NetworkCostType**](https://msdn.microsoft.com/library/windows/apps/br207292).

Aunque la característica de transferencia en segundo plano tiene sus propios mecanismos para controlar los cambios de estado de la red, hay otras consideraciones generales sobre conectividad que debes tener en cuenta en relación con las aplicaciones conectadas a la red. Si quieres obtener más información, lee el tema [Aprovechamiento de la información de conexión de red disponible](https://msdn.microsoft.com/library/windows/apps/hh452983).

> **Nota**para las aplicaciones que se ejecutan en dispositivos móviles, existen características que permiten al usuario supervisar y restringir la cantidad de datos que se transfieren en función del tipo de conexión, el estado de itinerancia y plan de datos del usuario. Por este motivo, las transferencias en segundo plano se pueden pausar en el teléfono, incluso cuando la [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) indica que la transferencia debe proseguir.

En esta tabla puedes ver cuándo se permiten transferencias en segundo plano en el teléfono para cada valor de [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) según el estado actual del teléfono. Puedes usar la clase [**ConnectionCost**](https://msdn.microsoft.com/library/windows/apps/br207244) para determinar el estado actual del teléfono.

| Estado del dispositivo                                                                                                                      | Solo sin restringir | Predeterminado | Siempre |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------|---------|--------|
| Conectado a Wi-Fi                                                                                                                 | Permitir            | Permitir   | Permitir  |
| Conexión de uso medido, sin roaming, con límite de datos, con seguimiento para permanecer por debajo del límite                                                   | Denegar             | Permitir   | Permitir  |
| Conexión de uso medido, sin roaming, con límite de datos, con seguimiento para superar el límite                                                       | Denegar             | Denegar    | Permitir  |
| Conexión de uso medido, roaming, con límite de datos                                                                                     | Denegar             | Denegar    | Permitir  |
| Conexión de uso medido, por encima del límite de datos. Este estado solo se produce cuando el usuario habilita "Restringir los datos en segundo plano" en la interfaz de usuario de Sensor de datos. | Denegar             | Denegar    | Denegar   |

## <a name="uploading-files"></a>Carga de archivos
Al usar la transferencia en segundo plano, la carga existe como una [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) que expone varios métodos de control que se usan para reiniciar o cancelar la operación. El sistema controla automáticamente los eventos de la aplicación (por ejemplo, suspensión o finalización) y los cambios en la conectividad por **UploadOperation**; las cargas continuarán durante los periodos de suspensión o pausa de la aplicación y se mantendrán tras la finalización de la misma. Además, al establecer la propiedad [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018), se indica si tu aplicación iniciará las cargas mientras se usa una red de uso medido para la conexión a Internet.

En los siguientes ejemplos, te explicaremos cómo crear e inicializar una carga básica, y cómo enumerar y volver a introducir operaciones persistentes de una sesión anterior de la aplicación.

### <a name="uploading-a-single-file"></a>Carga de un solo archivo
La creación de una carga comienza con [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140). Esta clase se usa para proporcionar los métodos que permiten a tu aplicación configurar la carga antes de crear la [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) resultante. En el siguiente ejemplo se muestra cómo realizar esto con los objetos [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) y [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) necesarios.

**Identificar el archivo y el destino de la carga**

Antes de comenzar la creación de una [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), debemos identificar el URI de la ubicación en la que se va a realizar la carga y el archivo que se cargará. En el siguiente ejemplo, el valor *uriString* se rellena mediante una cadena de entrada de UI y el valor *file* mediante el objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) devuelto por una operación [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275).

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_B "Identify the file and destination for the upload")]

**Crear e inicializar la operación de carga**

En el paso anterior, los valores *uriString* y *file* se pasan a una instancia de nuestro siguiente ejemplo, UploadOp, donde se usan para configurar e iniciar la nueva operación de carga. Primero, *uriString* se analiza para crear el objeto [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) necesario.

Después, [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) usa las propiedades del [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) proporcionado (*file*) para rellenar el encabezado de la solicitud y establecer la propiedad *SourceFile* con el objeto **StorageFile**. Después se llama al método [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146) para insertar el nombre de archivo, indicado como una cadena, y la propiedad [**StorageFile.Name**](https://msdn.microsoft.com/library/windows/apps/br227220).

Por último, [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) crea la [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) (*upload*).

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_A "Create and initialize the upload operation")]

Observa las llamadas de método asincrónico definidas con promesas de JavaScript. En una línea del último ejemplo:

```javascript
promise = upload.startAsync().then(complete, error, progress);
```

La llamada de método asincrónico está seguida por una instrucción then que indica métodos definidos por la aplicación a los que se llama cuando se devuelve un resultado de la llamada de método asincrónico. Si quieres obtener más información acerca de este patrón de programación, consulta el tema [Programación asincrónica en JavaScript con promesas](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx).

### <a name="uploading-multiple-files"></a>Carga de varios archivos
**Identificar los archivos y el destino de la carga**

En un escenario en el que se transfieran varios archivos con una sola [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), el proceso comienza como lo hace normalmente, proporcionando primero el URI de destino requerido y la información del archivo local. Al igual que en el ejemplo de la sección anterior, el usuario final proporciona el URI como una cadena y [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) se puede usar para ofrecer la capacidad de indicar archivos en la interfaz de usuario. Sin embargo, en este escenario la aplicación debería llamar en su lugar al método [**PickMultipleFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br207851) para permitir la selección de varios archivos mediante la interfaz de usuario.

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

Los siguientes dos ejemplos usan código contenido en un solo método de ejemplo, **startMultipart**, al que se ha llamado al final del último paso. Con fines instructivos, el código del método que crea una matriz de objetos [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) se ha separado del código que crea la [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) resultante.

En primer lugar, la cadena de URI proporcionada por el usuario se inicializa como [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998). A continuación, la matriz de objetos [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) (**archivos**) pasada a este método se itera y cada objeto se usa para crear un nuevo objeto [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029), el cual se coloca seguidamente en la matriz **contentParts**.

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

Con nuestra matriz contentParts compuesta por todos los objetos [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) que representan cada [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) para cargar, estamos listos para llamar a [**CreateUploadAsync**](https://msdn.microsoft.com/library/windows/apps/hh923973) con [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) para indicar dónde se enviará la solicitud.

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

### <a name="restarting-interrupted-upload-operations"></a>Reinicio de las operaciones de carga interrumpidas
Después de que se finalice o cancele una [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), todos los recursos del sistema asociados a ella se liberan. Sin embargo, si tu aplicación finaliza antes de que sucedan algunas de estas situaciones, se pausan todas las operaciones activas y se mantienen ocupados los recursos asociados a ellas. Si no se enumeran estas operaciones y se vuelven a introducir en la siguiente sesión de la aplicación, no se completarán y seguirán ocupando recursos del dispositivo.

1.  Antes de definir la función que enumera las operaciones persistentes, necesitamos crear una matriz que contenga los objetos [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) que va a devolver:

    [!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_C "Restart interrupted upload operation")]

1.  Después definimos la función que enumera las operaciones persistentes y las almacena en nuestra matriz. Ten en cuenta que el método **load** al que se llamó para reasignar las devoluciones de llamada a la [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), si es que persiste tras la finalización de la aplicación, se encuentra en la clase UploadOp que definiremos más adelante en esta sección.

    [!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_D "Enumerate persisted operations")]

## <a name="downloading-files"></a>Descarga de archivos
Al usar la transferencia en segundo plano, cada descarga existe como una [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) que expone varios métodos de control que se usan para pausar, reanudar, reiniciar o cancelar la operación. El sistema controla automáticamente los eventos de la aplicación (por ejemplo, suspensión o finalización) y los cambios en la conectividad por **DownloadOperation**; las descargas continuarán durante los periodos de suspensión o pausa de la aplicación y se mantendrán tras la finalización de la misma. En escenarios de red móvil, al establecer la propiedad [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018), se indica si tu aplicación iniciará o continuará las descargas mientras se usa una red de uso medido para la conexión a Internet.

Si descargas recursos pequeños que probablemente se completen rápidamente, debes usar las API [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) en vez de una transferencia en segundo plano.

En los siguientes ejemplos, te explicaremos cómo crear e inicializar una descarga básica y cómo enumerar y volver a introducir operaciones persistentes de una sesión anterior de la aplicación.

### <a name="configure-and-start-a-background-transfer-file-download"></a>Configurar e iniciar una descarga de archivos mediante transferencia en segundo plano
El siguiente ejemplo demuestra cómo las cadenas que representan un URI y un nombre de archivo pueden usarse para crear un objeto [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) y el [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que contendrá el recurso solicitado. En este ejemplo, el nuevo archivo se coloca automáticamente en una ubicación predefinida. Como alternativa, se puede usar [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) para permitir que los usuarios indiquen dónde guardar el archivo en el dispositivo. Ten en cuenta que el método **load** al que se llamó para reasignar las devoluciones de llamada a la [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154), si es que persiste tras la finalización de la aplicación, se encuentra en la clase DownloadOp, que definiremos más adelante en esta sección.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_A)]

Observa las llamadas de método asincrónico definidas con promesas de JavaScript. En la línea 17 del ejemplo de código anterior:

```javascript
promise = download.startAsync().then(complete, error, progress);
```

La llamada de método asincrónico está seguida por una instrucción then que indica métodos definidos por la aplicación a los que se llama cuando se devuelve un resultado de la llamada de método asincrónico. Si quieres obtener más información acerca de este patrón de programación, consulta el tema [Programación asincrónica en JavaScript con promesas](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx).

### <a name="adding-additional-operation-control-methods"></a>Agregar métodos adicionales de control de operaciones
Se puede aumentar el nivel de control implementando métodos [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) adicionales. Por ejemplo, si se agrega el siguiente código al ejemplo anterior, se introduce la capacidad de cancelar la descarga.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_B)]

### <a name="enumerating-persisted-operations-at-start-up"></a>Enumeración de operaciones persistentes en el inicio
Después de finalizada o cancelada una [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154), se liberan todos los recursos del sistema asociados a ella. Sin embargo, si tu aplicación finaliza antes de que se produzca alguno de estos eventos, se pausarán las descargas y se mantendrán en segundo plano. Los siguientes ejemplos demuestran cómo volver a introducir descargas persistentes en una nueva sesión de la aplicación.

1.  Antes de definir la función que enumera las operaciones persistentes, necesitamos crear una matriz que contenga los objetos [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) que va a devolver:

    [!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_D)]

1.  Después definimos la función que enumera las operaciones persistentes y las almacena en nuestra matriz. Ten en cuenta que el método **load** al que se llamó para reasignar las devoluciones de llamada para una [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) persistente se encuentra en el ejemplo de DownloadOp que definiremos más adelante en esta sección.

    [!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_E)]

1.  Ahora puedes usar la lista rellenada para reiniciar las operaciones pendientes.

## <a name="post-processing"></a>Posprocesamiento
Una nueva característica de Windows 10 es la capacidad para ejecutar código de la aplicación tras la finalización de una transferencia en segundo plano, incluso cuando no se está ejecutando la aplicación. Por ejemplo, puede que la aplicación quiera actualizar una lista de películas disponibles una vez finalizada la descarga, en lugar de que la aplicación busque películas nuevas cada vez que se inicie. O puede que la aplicación quiera administrar una transferencia de archivos errónea intentando usar de nuevo un servidor o puerto diferentes. Se invoca un posprocesamiento para transferencias correctas y fallidas, de modo que pueda usarlo para implementar el control de errores y la lógica de reintentos personalizados.

El posprocesamiento usa la infraestructura de tareas en segundo plano existente. Creas una tarea en segundo plano y la asocias con tus transferencias antes de iniciarlas. Después, se ejecutan las transferencias en segundo plano y, una vez completadas, se llama a la tarea en segundo plano para realizar un posprocesamiento.

El posprocesamiento usa una clase nueva, [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209). Esta clase es similar al [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030) existente que permite agrupar transferencias en segundo plano, si bien el **BackgroundTransferCompletionGroup** incluye la posibilidad de designar la ejecución de una tarea en segundo plano cuando se complete la transferencia.

Puedes iniciar una transferencia en segundo plano con posprocesamiento de la siguiente manera.

1.  Crea un objeto [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209). A continuación, crea un objeto [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Establece la propiedad **Trigger** del objeto de generador para el objeto de grupo de finalización y la propiedad **TaskEntryPoint** del generador para el punto de entrada de la tarea en segundo plano que se debe ejecutar al finalizar la transferencia. Por último, llama al método [**BackgroundTaskBuilder.Register**](https://msdn.microsoft.com/library/windows/apps/br224772) para registrar la tarea en segundo plano. Ten en cuenta que muchos de los grupos de finalización pueden compartir un punto de entrada de tarea en segundo plano, pero solo puedes tener un grupo de finalización por registro de tareas en segundo plano.

```csharp
var completionGroup = new BackgroundTransferCompletionGroup();
BackgroundTaskBuilder builder = new BackgroundTaskBuilder();

builder.Name = "MyDownloadProcessingTask";
builder.SetTrigger(completionGroup.Trigger);
builder.TaskEntryPoint = "Tasks.BackgroundDownloadProcessingTask";

BackgroundTaskRegistration downloadProcessingTask = builder.Register();
```

2.  A continuación, asocia transferencias en segundo plano con el grupo de finalización. Una vez creadas todas las transferencias, habilita el grupo de finalización.

```csharp
BackgroundDownloader downloader = new BackgroundDownloader(completionGroup);
DownloadOperation download = downloader.CreateDownload(uri, file);
Task<DownloadOperation> startTask = download.StartAsync().AsTask();

// App still sees the normal completion path
startTask.ContinueWith(ForegroundCompletionHandler);

// Do not enable the CompletionGroup until after all downloads are created.
downloader.CompletinGroup.Enable();
```

3.  El código de la tarea en segundo plano extrae la lista de operaciones de los detalles del desencadenador y, a continuación, el código puede inspeccionar los detalles de cada operación y realizar el correspondiente posprocesamiento.

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

La tarea de posprocesamiento es una tarea en segundo plano normal. Forma parte del grupo de todas las tareas en segundo plano y está sujeta a la misma directiva de administración de recursos que todas ellas.

Además, ten en cuenta que el posprocesamiento no reemplaza a los controladores de finalización en primer plano. Si la aplicación define un controlador de finalización en primer plano y la aplicación se ejecuta cuando se completa la transferencia de archivos, se llamará a los controladores de finalización de ambos planos. No se garantiza el orden en que se llame a las tareas en primer y en segundo plano. Si definiste ambas, debes asegurarte de que las dos tareas funcionarán correctamente y no interferirán entre sí si se ejecutan al mismo tiempo.

## <a name="request-timeouts"></a>Tiempos de espera de solicitudes
Son dos los escenarios principales relacionados con el tiempo de espera de la conexión que se deben tener en cuenta:

-   Al establecer una nueva conexión para una transferencia, la solicitud de conexión se cancelará si no se establece en cinco minutos.

-   Después de establecer una conexión, se cancelará el mensaje de solicitud HTTP que no haya recibido una respuesta transcurridos dos minutos.

> **Nota**en ambos escenarios, y suponiendo que haya conectividad a Internet, transferencia en segundo plano reintentará una solicitud hasta tres veces automáticamente. Si no se detecta una conexión a Internet, las solicitudes adicionales esperarán hasta que se establezca la conexión.

## <a name="debugging-guidance"></a>Guía para la depuración
Detener una sesión de depuración en Microsoft Visual Studio se puede comparar a cerrar una aplicación. Las cargas PUT se pausan y las cargas POST se finalizan. Incluso durante la depuración, tu aplicación debe enumerar y reiniciar o cancelar todas las cargas que persisten. Por ejemplo, tu aplicación puede cancelar operaciones de carga persistentes al iniciar la aplicación, si no hay ningún interés por operaciones anteriores para la sesión de depuración.

Mientras se enumeran las cargas y descargas al iniciarse una aplicación durante una sesión de depuración, puedes hacer que tu aplicación las cancele si, en esa sesión de depuración, no se tiene ningún interés por las operaciones anteriores. Ten en cuenta que si hay actualizaciones de proyectos de Visual Studio, como cambios en el manifiesto de la aplicación, y la aplicación se desinstala y se vuelve a instalar, [**GetCurrentUploadsAsync**](https://msdn.microsoft.com/library/windows/apps/hh701149) no podrá enumerar las operaciones creadas con la implementación de aplicación anterior.

Cuando uses la transferencia en segundo plano durante el desarrollo, es posible que las memorias caché internas de las operaciones de transferencia activas y finalizadas dejen de estar sincronizadas. Esto puede provocar que no puedas iniciar nuevas operaciones de transferencia ni interactuar con operaciones existentes y objetos [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030). En algunos casos, intentar interactuar con operaciones ya existentes puede producir un bloqueo. Este resultado puede producirse si la propiedad [**TransferBehavior**](https://msdn.microsoft.com/library/windows/apps/dn279033) se establece en **Parallel**. Este problema se produce únicamente en determinados escenarios, durante el desarrollo, y no se aplica a los usuarios finales de tu aplicación.

Cuatro escenarios en los que se usa Visual Studio pueden producir este problema:

-   Creas un proyecto nuevo con el mismo nombre de aplicación que un proyecto ya existente, pero otro lenguaje (de C++ a C#, por ejemplo).
-   Cambias la arquitectura de destino (de x86 a x64, por ejemplo) de un proyecto ya existente.
-   Cambias la cultura (de neutral a en-US, por ejemplo) en un proyecto ya existente.
-   Agregas o quitas una capacidad en el manifiesto del paquete (agregando, por ejemplo, **Autenticación de empresa**) en un proyecto ya existente.

El mantenimiento regular de tu aplicación, incluidas las actualizaciones de manifiesto que agregan o quitan capacidades, no causa este problema en las implementaciones de usuario final de tu aplicación.
Para evitar este problema, desinstala completamente todas las versiones de la aplicación y vuelve a implementarlas con el lenguaje, la arquitectura, la cultura o la capacidad nuevos. Puedes hacerlo a través de la pantalla **Inicio** o usando PowerShell y el cmdlet **Remove-AppxPackage**.

## <a name="exceptions-in-windowsnetworkingbackgroundtransfer"></a>Excepciones en Windows.Networking.BackgroundTransfer
Se inicia una excepción cuando se pasa una cadena no válida del identificador uniforme de recursos (URI) al constructor del objeto [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998).

**.NET:** El tipo [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) aparece como [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) en C# y VB.

En C# y Visual Basic, este error puede evitarse si se usa la clase [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) en .NET 4.5 y uno de los métodos [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) para probar la cadena recibida del usuario de la aplicación antes de que se cree el URI.

En C++, no hay ningún método para probar y analizar una cadena en un URI. Si una aplicación recibe información del usuario relativa a [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998), el constructor debe estar en un bloque Try/Catch. Si se genera una excepción, la aplicación podrá notificar al usuario y solicitar otro nombre de host.

El espacio de nombres [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) tiene prácticos métodos de ayuda y usa enumeraciones en el espacio de nombres [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) para administrar los errores. Esto puede ser útil para controlar de un modo diferente excepciones de red específicas en la aplicación.

Un error encontrado en un método asincrónico en el espacio de nombres [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) se devuelve como un valor **HRESULT**. El método [**BackgroundTransferError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701093) se usa para convertir un error de red de una operación de transferencia en segundo plano en un valor de enumeración [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818). La mayoría de los valores de enumeración **WebErrorStatus** corresponden a un error devuelto por la operación de cliente HTTP o FTP nativa. Una aplicación puede filtrar según valores **WebErrorStatus** concretos para modificar el comportamiento de la aplicación en función del motivo de la excepción.

Para los errores de validación de parámetros, una aplicación también puede usar el **HRESULT** de la excepción para obtener información más detallada del error que causó la excepción. Los valores posibles de **HRESULT** se enumeran en el archivo de encabezado *Winerror.h*. Para la mayoría de los errores de validación de parámetros, el valor de **HRESULT** devuelto es **E\_INVALIDARG**.

## <a name="important-apis"></a>API importantes
* [**Windows.Networking.BackgroundTransfer**](/uwp/api/windows.networking.backgroundtransfer)
* [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri)
* [**Windows.Networking.Sockets**](/uwp/api/Windows.Networking.Sockets)
