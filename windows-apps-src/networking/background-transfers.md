---
description: Usa la API de transferencia en segundo plano para copiar archivos de forma confiable en la red.
title: Transferencias en segundo plano
ms.assetid: 1207B089-BC16-4BF0-BBD4-FD99950C764B
ms.date: 03/23/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: faf88751f5008c5a819bb39bb461a4224180edf2
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363018"
---
# <a name="background-transfers"></a>Transferencias en segundo plano
Usa la API de transferencia en segundo plano para copiar archivos de forma confiable en la red. La API de transferencia en segundo plano proporciona funciones de carga y descarga avanzadas que se ejecutan en segundo plano durante la suspensión de la aplicación y que persisten tras la finalización de esta. La API supervisa el estado de red, y suspende y reanuda automáticamente las transferencias cuando se pierde la conexión. Además, las transferencias son compatibles con el sensor de datos y el de batería, lo que significa que la actividad de descarga se ajusta según la conectividad y el estado de la batería actuales del dispositivo. La API es ideal para cargar y descargar archivos grandes mediante HTTP(S). También se admite FTP, pero solo para descargas.

La transferencia en segundo plano se ejecuta con independencia de la aplicación que llama y está diseñada principalmente para operaciones de transferencia a largo plazo para recursos como vídeo, música e imágenes de gran tamaño. Para estos escenarios, el uso de transferencias en segundo plano es fundamental, ya que las descargas siguen progresando, incluso aunque se suspenda la aplicación.

Si descargas recursos pequeños que probablemente se completen rápidamente, debes usar las API [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) en vez de una transferencia en segundo plano.

## <a name="using-windowsnetworkingbackgroundtransfer"></a>Uso de Windows.Networking.BackgroundTransfer

### <a name="how-does-the-background-transfer-feature-work"></a>¿Cómo funciona la característica de transferencia en segundo plano?
Cuando una aplicación usa una transferencia en segundo plano para iniciar una transferencia, la solicitud se configura e inicializa con objetos de clase [**BackgroundDownloader**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundDownloader) o [**BackgroundUploader**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundUploader). El sistema controla cada operación de transferencia por separado y la separa de la aplicación que llama. Dispones de información del progreso si quieres indicar el estado del usuario en la interfaz de usuario de la aplicación. Tu aplicación puede pausar, reanudar, cancelar o incluso leer los datos mientras se realiza la transferencia. La manera en que el sistema controla las transferencias promueve el uso inteligente de energía y evita los problemas que pueden surgir ante eventos que afectan a la aplicación conectada, como la suspensión inesperada de la aplicación, la finalización o cambios repentinos en el estado de red.

> [!NOTE]
> Debido a restricciones de recursos por aplicación, una aplicación no debe tener más de 200 transferencias (DownloadOperations + UploadOperations) en cualquier momento. La superación de ese límite puede dejar la cola de transferencia de la aplicación en un estado irrecuperable.

Cuando se inicia una aplicación, debe llamar a [**AttachAsync**](/uwp/api/windows.networking.backgroundtransfer.downloadoperation.AttachAsync) en todos los objetos [**DownloadOperation**](/uwp/api/windows.networking.backgroundtransfer.downloadoperation) y [ **UploadOperation**](/uwp/api/windows.networking.backgroundtransfer.uploadoperation) existentes. Si no se hace, provocará la pérdida de las transferencias ya completadas y, en definitiva, que sea inútil el uso de la característica de transferencia en segundo plano.

### <a name="performing-authenticated-file-requests-with-background-transfer"></a>Realización de solicitudes de archivos autenticadas con la transferencia en segundo plano
La transferencia en segundo plano proporciona métodos que admiten credenciales básicas de proxy y servidor, así como el uso de encabezados HTTP personalizados (mediante [**SetRequestHeader**](/uwp/api/windows.networking.backgroundtransfer.backgrounduploader.setrequestheader)) para cada operación de transferencia.

### <a name="how-does-this-feature-adapt-to-network-status-changes-or-unexpected-shutdowns"></a>¿Cómo se adapta esta característica a los cambios de estado de red o apagados inesperados?
Cuando se producen cambios en el estado de la red, la transferencia en segundo plano mantiene una experiencia coherente para cada operación de transferencia, ya que saca provecho de manera inteligente de la información sobre el estado de la conectividad y el plan de datos del operador, que se proporciona con la característica [Conectividad](/previous-versions/windows/apps/hh452990(v=win.10)). Para definir el comportamiento en distintos escenarios de red, una aplicación establece una directiva de coste para cada operación usando los valores que [**BackgroundTransferCostPolicy**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferCostPolicy) define.

Por ejemplo, la directiva de costo definida para una operación puede indicar que se debe pausar automáticamente la operación cuando el dispositivo está usando una red de uso medido. La transferencia se reanudará (o reiniciará) de forma automática cuando se establezca una conexión a una red "sin restricciones". Para obtener más información sobre cómo se definen las redes por coste, consulta [**NetworkCostType**](/uwp/api/Windows.Networking.Connectivity.NetworkCostType).

Aunque la característica de transferencia en segundo plano tiene sus propios mecanismos para controlar los cambios de estado de la red, hay otras consideraciones generales sobre conectividad que debes tener en cuenta en relación con las aplicaciones conectadas a la red. Si quieres obtener más información, lee el tema [Aprovechamiento de la información de conexión de red disponible](/previous-versions/windows/apps/hh452983(v=win.10)).

> **Nota**   En el caso de las aplicaciones que se ejecutan en dispositivos móviles, existen características que permiten al usuario supervisar y restringir la cantidad de datos que se transfieren, en función del tipo de conexión, el estado de itinerancia y el plan de datos del usuario. Por este motivo, las transferencias en segundo plano se pueden pausar en el teléfono, incluso cuando la [**BackgroundTransferCostPolicy**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferCostPolicy) indica que la transferencia debe proseguir.

En esta tabla puedes ver cuándo se permiten transferencias en segundo plano en el teléfono para cada valor de [**BackgroundTransferCostPolicy**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferCostPolicy) según el estado actual del teléfono. Puedes usar la clase [**ConnectionCost**](/uwp/api/Windows.Networking.Connectivity.ConnectionCost) para determinar el estado actual del teléfono.

| Estado del dispositivo                                                                                                                      | Solo sin restringir | Valor predeterminado | Siempre |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------|---------|--------|
| Conectado a Wi-Fi                                                                                                                 | Permitir            | Permitir   | Permitir  |
| Conexión de uso medido, sin roaming, con límite de datos, con seguimiento para permanecer por debajo del límite                                                   | Denegar             | Permitir   | Permitir  |
| Conexión de uso medido, sin roaming, con límite de datos, con seguimiento para superar el límite                                                       | Denegar             | Denegar    | Permitir  |
| Conexión de uso medido, roaming, con límite de datos                                                                                     | Denegar             | Denegar    | Permitir  |
| Conexión de uso medido, por encima del límite de datos. Este estado solo se produce cuando el usuario habilita "Restringir los datos en segundo plano" en la interfaz de usuario de Sensor de datos. | Denegar             | Denegar    | Denegar   |

## <a name="uploading-files"></a>Carga de archivos
Al usar la transferencia en segundo plano, la carga existe como una [**UploadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) que expone varios métodos de control que se usan para reiniciar o cancelar la operación. El sistema controla automáticamente los eventos de la aplicación (por ejemplo, la suspensión o la finalización) y los cambios en la conectividad por **UploadOperation**; las cargas continuarán durante los periodos de suspensión o pausa de la aplicación y se mantendrán tras la finalización de esta. Además, al establecer la propiedad [**CostPolicy**](/uwp/api/windows.networking.backgroundtransfer.backgrounddownloader.costpolicy), se indica si tu aplicación iniciará las cargas mientras se usa una red de uso medido para la conexión a Internet.

En los siguientes ejemplos, te explicaremos cómo crear e inicializar una carga básica, y cómo enumerar y volver a introducir operaciones persistentes de una sesión anterior de la aplicación.

### <a name="uploading-a-single-file"></a>Carga de un solo archivo
La creación de una carga comienza con [**BackgroundUploader**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundUploader). Esta clase se usa para proporcionar los métodos que permiten a tu aplicación configurar la carga antes de crear la [**UploadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) resultante. En el siguiente ejemplo se muestra cómo realizar esto con los objetos [**Uri**](/uwp/api/Windows.Foundation.Uri) y [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) necesarios.

**Identificar el archivo y el destino de la carga**

Antes de comenzar la creación de una [**UploadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation), debemos identificar el URI de la ubicación en la que se va a realizar la carga y el archivo que se cargará. En el siguiente ejemplo, el valor *uriString* se rellena mediante una cadena de entrada de UI y el valor *file* mediante el objeto [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) devuelto por una operación [**PickSingleFileAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync).

:::code language="javascript" source="~/../snippets-windows/windows-uwp/networking/backgroundtransfer/upload_quickstart/js/main.js" id="Snippetupload_quickstart_B":::

**Crear e inicializar la operación de carga**

En el paso anterior, los valores *uriString* y *file* se pasan a una instancia de nuestro siguiente ejemplo, UploadOp, donde se usan para configurar e iniciar la nueva operación de carga. Primero, *uriString* se analiza para crear el objeto [**Uri**](/uwp/api/Windows.Foundation.Uri) necesario.

Después, [**BackgroundUploader**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundUploader) usa las propiedades del [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) proporcionado (*file*) para rellenar el encabezado de la solicitud y establecer la propiedad *SourceFile* con el objeto **StorageFile**. Después se llama al método [**SetRequestHeader**](/uwp/api/windows.networking.backgroundtransfer.backgrounduploader.setrequestheader) para insertar el nombre de archivo, indicado como una cadena, y la propiedad [**StorageFile.Name**](/uwp/api/windows.storage.storagefile.name).

Por último, [**BackgroundUploader**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundUploader) crea la [**UploadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) (*upload*).

:::code language="javascript" source="~/../snippets-windows/windows-uwp/networking/backgroundtransfer/upload_quickstart/js/main.js" id="Snippetupload_quickstart_A":::

Observa las llamadas de método asincrónico definidas con promesas de JavaScript. En una línea del último ejemplo:

```javascript
promise = upload.startAsync().then(complete, error, progress);
```

La llamada de método asincrónico está seguida por una instrucción then que indica métodos definidos por la aplicación a los que se llama cuando se devuelve un resultado de la llamada de método asincrónico. Si quieres obtener más información acerca de este patrón de programación, consulta el tema [Programación asincrónica en JavaScript con promesas](/previous-versions/windows).

### <a name="uploading-multiple-files"></a>Carga de varios archivos
**Identificar los archivos y el destino de la carga**

En un escenario en el que se transfieran varios archivos con una sola [**UploadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation), el proceso comienza como lo hace normalmente, proporcionando primero el URI de destino requerido y la información del archivo local. Al igual que en el ejemplo de la sección anterior, el usuario final proporciona el URI como una cadena y [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) se puede usar para ofrecer la capacidad de indicar archivos en la interfaz de usuario. Sin embargo, en este escenario la aplicación debería llamar en su lugar al método [**PickMultipleFilesAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.pickmultiplefilesasync) para permitir la selección de varios archivos mediante la interfaz de usuario.

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

Los siguientes dos ejemplos usan código contenido en un solo método de ejemplo, **startMultipart**, al que se ha llamado al final del último paso. Con fines instructivos, el código del método que crea una matriz de objetos [**BackgroundTransferContentPart**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart) se ha separado del código que crea la [**UploadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) resultante.

En primer lugar, la cadena de URI proporcionada por el usuario se inicializa como [**Uri**](/uwp/api/Windows.Foundation.Uri). A continuación, la matriz de objetos [**IStorageFile**](/uwp/api/Windows.Storage.IStorageFile) (**archivos**) pasada a este método se itera y cada objeto se usa para crear un nuevo objeto [**BackgroundTransferContentPart**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart), el cual se coloca seguidamente en la matriz **contentParts**.

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

Con nuestra matriz contentParts compuesta por todos los objetos [**BackgroundTransferContentPart**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart) que representan cada [**IStorageFile**](/uwp/api/Windows.Storage.IStorageFile) para cargar, estamos listos para llamar a [**CreateUploadAsync**](/uwp/api/windows.networking.backgroundtransfer.backgrounduploader.createuploadasync) con [**Uri**](/uwp/api/Windows.Foundation.Uri) para indicar dónde se enviará la solicitud.

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
Después de que se finalice o cancele una [**UploadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation), todos los recursos del sistema asociados a ella se liberan. Sin embargo, si tu aplicación finaliza antes de que sucedan algunas de estas situaciones, se pausan todas las operaciones activas y se mantienen ocupados los recursos asociados a ellas. Si no se enumeran estas operaciones y se vuelven a introducir en la siguiente sesión de la aplicación, no se completarán y seguirán ocupando recursos del dispositivo.

1.  Antes de definir la función que enumera las operaciones persistentes, necesitamos crear una matriz que contenga los objetos [**UploadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) que va a devolver:

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/networking/backgroundtransfer/upload_quickstart/js/main.js" id="Snippetupload_quickstart_C":::

1.  Después definimos la función que enumera las operaciones persistentes y las almacena en nuestra matriz. Ten en cuenta que el método **load** al que se llamó para reasignar las devoluciones de llamada a la [**UploadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation), si es que persiste tras la finalización de la aplicación, se encuentra en la clase UploadOp que definiremos más adelante en esta sección.

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/networking/backgroundtransfer/upload_quickstart/js/main.js" id="Snippetupload_quickstart_D":::

## <a name="downloading-files"></a>Descarga de archivos
Al usar la transferencia en segundo plano, cada descarga existe como una [**DownloadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation) que expone varios métodos de control que se usan para pausar, reanudar, reiniciar o cancelar la operación. El sistema controla automáticamente los eventos de la aplicación (por ejemplo, la suspensión o la finalización) y los cambios en la conectividad por **DownloadOperation**; las descargas continuarán durante los periodos de suspensión o pausa de la aplicación y se mantendrán tras la finalización de esta. En escenarios de red móvil, al establecer la propiedad [**CostPolicy**](/uwp/api/windows.networking.backgroundtransfer.backgrounddownloader.costpolicy), se indica si tu aplicación iniciará o continuará las descargas mientras se usa una red de uso medido para la conexión a Internet.

Si descargas recursos pequeños que probablemente se completen rápidamente, debes usar las API [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) en vez de una transferencia en segundo plano.

En los siguientes ejemplos, te explicaremos cómo crear e inicializar una descarga básica y cómo enumerar y volver a introducir operaciones persistentes de una sesión anterior de la aplicación.

### <a name="configure-and-start-a-background-transfer-file-download"></a>Configurar e iniciar una descarga de archivos mediante transferencia en segundo plano
El siguiente ejemplo demuestra cómo las cadenas que representan un URI y un nombre de archivo pueden usarse para crear un objeto [**Uri**](/uwp/api/Windows.Foundation.Uri) y el [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) que contendrá el recurso solicitado. En este ejemplo, el nuevo archivo se coloca automáticamente en una ubicación predefinida. Como alternativa, se puede usar [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) para permitir que los usuarios indiquen dónde guardar el archivo en el dispositivo. Ten en cuenta que el método **load** al que se llamó para reasignar las devoluciones de llamada a la [**DownloadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation), si es que persiste tras la finalización de la aplicación, se encuentra en la clase DownloadOp, que definiremos más adelante en esta sección.

:::code language="javascript" source="~/../snippets-windows/windows-uwp/networking/backgroundtransfer/download_quickstart/js/main.js" id="Snippetdownload_quickstart_A":::

Observa las llamadas de método asincrónico definidas con promesas de JavaScript. En la línea 17 del ejemplo de código anterior:

```javascript
promise = download.startAsync().then(complete, error, progress);
```

La llamada de método asincrónico está seguida por una instrucción then que indica métodos definidos por la aplicación a los que se llama cuando se devuelve un resultado de la llamada de método asincrónico. Si quieres obtener más información acerca de este patrón de programación, consulta el tema [Programación asincrónica en JavaScript con promesas](/previous-versions/windows).

### <a name="adding-additional-operation-control-methods"></a>Agregar métodos adicionales de control de operaciones
Se puede aumentar el nivel de control implementando métodos [**DownloadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation) adicionales. Por ejemplo, si se agrega el siguiente código al ejemplo anterior, se introduce la capacidad de cancelar la descarga.

:::code language="javascript" source="~/../snippets-windows/windows-uwp/networking/backgroundtransfer/download_quickstart/js/main.js" id="Snippetdownload_quickstart_B":::

### <a name="enumerating-persisted-operations-at-start-up"></a>Enumeración de operaciones persistentes en el inicio
Después de finalizada o cancelada una [**DownloadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation), se liberan todos los recursos del sistema asociados a ella. Sin embargo, si tu aplicación finaliza antes de que se produzca alguno de estos eventos, se pausarán las descargas y se mantendrán en segundo plano. Los siguientes ejemplos demuestran cómo volver a introducir descargas persistentes en una nueva sesión de la aplicación.

1.  Antes de definir la función que enumera las operaciones persistentes, necesitamos crear una matriz que contenga los objetos [**DownloadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation) que va a devolver:

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/networking/backgroundtransfer/download_quickstart/js/main.js" id="Snippetdownload_quickstart_D":::

1.  Después definimos la función que enumera las operaciones persistentes y las almacena en nuestra matriz. Ten en cuenta que el método **load** al que se llamó para reasignar las devoluciones de llamada para una [**DownloadOperation**](/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation) persistente se encuentra en el ejemplo de DownloadOp que definiremos más adelante en esta sección.

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/networking/backgroundtransfer/download_quickstart/js/main.js" id="Snippetdownload_quickstart_E":::

1.  Ahora puedes usar la lista rellenada para reiniciar las operaciones pendientes.

## <a name="post-processing"></a>Posprocesamiento
Una nueva característica de Windows 10 es la capacidad de ejecutar código de la aplicación tras la finalización de una transferencia en segundo plano incluso cuando la aplicación no se está ejecutando. Por ejemplo, puede que la aplicación quiera actualizar una lista de películas disponibles una vez finalizada la descarga, en lugar de que la aplicación busque películas nuevas cada vez que se inicie. O puede que la aplicación quiera administrar una transferencia de archivos errónea intentando usar de nuevo un servidor o puerto diferentes. Se invoca un posprocesamiento para transferencias correctas y fallidas, de modo que pueda usarlo para implementar el control de errores y la lógica de reintentos personalizados.

El posprocesamiento usa la infraestructura de tareas en segundo plano existente. Creas una tarea en segundo plano y la asocias con tus transferencias antes de iniciarlas. Después, se ejecutan las transferencias en segundo plano y, una vez completadas, se llama a la tarea en segundo plano para realizar un posprocesamiento.

El posprocesamiento usa una clase nueva, [**BackgroundTransferCompletionGroup**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferCompletionGroup). Esta clase es similar al [**BackgroundTransferGroup**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferGroup) existente que permite agrupar transferencias en segundo plano, si bien el **BackgroundTransferCompletionGroup** incluye la posibilidad de designar la ejecución de una tarea en segundo plano cuando se complete la transferencia.

Puedes iniciar una transferencia en segundo plano con posprocesamiento de la siguiente manera.

1.  Crea un objeto [**BackgroundTransferCompletionGroup**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferCompletionGroup). A continuación, crea un objeto [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder). Establece la propiedad **Trigger** del objeto de generador en el objeto de grupo de finalización y la propiedad **TaskEntryPoint** del generador en el punto de entrada de la tarea en segundo plano que se debe ejecutar al finalizar la transferencia. Por último, llama al método [**BackgroundTaskBuilder.Register**](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register) para registrar la tarea en segundo plano. Ten en cuenta que muchos de los grupos de finalización pueden compartir un punto de entrada de tarea en segundo plano, pero solo puedes tener un grupo de finalización por registro de tareas en segundo plano.

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

> **Nota**  En ambos escenarios, y suponiendo que hay conexión a Internet, la transferencia en segundo plano intentará una solicitud hasta tres veces de manera automática. Si no se detecta una conexión a Internet, las solicitudes adicionales esperarán hasta que se establezca la conexión.

## <a name="debugging-guidance"></a>Guía para la depuración
Detener una sesión de depuración en Microsoft Visual Studio se puede comparar a cerrar una aplicación. Las cargas PUT se pausan y las cargas POST se finalizan. Incluso durante la depuración, tu aplicación debe enumerar y reiniciar o cancelar todas las cargas que persisten. Por ejemplo, tu aplicación puede cancelar operaciones de carga persistentes al iniciar la aplicación, si no hay ningún interés por operaciones anteriores para la sesión de depuración.

Mientras se enumeran las cargas y descargas al iniciarse una aplicación durante una sesión de depuración, puedes hacer que tu aplicación las cancele si, en esa sesión de depuración, no se tiene ningún interés por las operaciones anteriores. Ten en cuenta que si hay actualizaciones de proyectos de Visual Studio, como cambios en el manifiesto de la aplicación, y la aplicación se desinstala y se vuelve a instalar, [**GetCurrentUploadsAsync**](/uwp/api/windows.networking.backgroundtransfer.backgrounduploader.getcurrentuploadsasync) no podrá enumerar las operaciones creadas con la implementación de aplicación anterior.

Cuando uses la transferencia en segundo plano durante el desarrollo, es posible que las memorias caché internas de las operaciones de transferencia activas y finalizadas dejen de estar sincronizadas. Esto puede provocar que no puedas iniciar nuevas operaciones de transferencia ni interactuar con operaciones existentes y objetos de [**BackgroundTransferGroup**](/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferGroup). En algunos casos, intentar interactuar con operaciones ya existentes puede producir un bloqueo. Este resultado puede producirse si la propiedad [**TransferBehavior**](/uwp/api/windows.networking.backgroundtransfer.backgroundtransfergroup.transferbehavior) se establece en **Parallel**. Este problema se produce únicamente en determinados escenarios, durante el desarrollo, y no se aplica a los usuarios finales de tu aplicación.

Cuatro escenarios en los que se usa Visual Studio pueden producir este problema:

-   Creas un proyecto nuevo con el mismo nombre de aplicación que un proyecto ya existente, pero otro lenguaje (de C++ a C#, por ejemplo).
-   Cambias la arquitectura de destino (de x86 a x64, por ejemplo) de un proyecto ya existente.
-   Cambias la cultura (de neutral a en-US, por ejemplo) en un proyecto ya existente.
-   Agregas o quitas una capacidad en el manifiesto del paquete (agregando, por ejemplo, **Autenticación de empresa**) en un proyecto ya existente.

El mantenimiento regular de tu aplicación, incluidas las actualizaciones de manifiesto que agregan o quitan capacidades, no causa este problema en las implementaciones de usuario final de tu aplicación.
Para evitar este problema, desinstala completamente todas las versiones de la aplicación y vuelve a implementarlas con el lenguaje, la arquitectura, la cultura o la capacidad nuevos. Puedes hacerlo a través de la pantalla **Inicio** o usando PowerShell y el cmdlet **Remove-AppxPackage**.

## <a name="exceptions-in-windowsnetworkingbackgroundtransfer"></a>Excepciones en Windows.Networking.BackgroundTransfer
Se inicia una excepción cuando se pasa una cadena no válida del identificador uniforme de recursos (URI) al constructor del objeto [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri).

**.NET:** El tipo [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri) aparece como [**System.Uri**](/dotnet/api/system.uri) en C# y VB.

En C# y Visual Basic, este error puede evitarse si se usa la clase [**System.Uri**](/dotnet/api/system.uri) en .NET 4.5 y uno de los métodos [**System.Uri.TryCreate**](/dotnet/api/system.uri.trycreate#overloads) para probar la cadena recibida del usuario de la aplicación antes de que se cree el URI.

En C++, no hay ningún método para probar y analizar una cadena en un URI. Si una aplicación recibe información del usuario relativa a [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri), el constructor debe estar en un bloque Try/Catch. Si se genera una excepción, la aplicación podrá notificar al usuario y solicitar otro nombre de host.

El espacio de nombres [**Windows.Networking.backgroundTransfer**](/uwp/api/Windows.Networking.BackgroundTransfer) tiene prácticos métodos de ayuda y usa enumeraciones en el espacio de nombres [**Windows.Networking.Sockets**](/uwp/api/Windows.Networking.Sockets) para administrar los errores. Esto puede ser útil para controlar de un modo diferente excepciones de red específicas en la aplicación.

Un error encontrado en un método asincrónico en el espacio de nombres [**Windows.Networking.backgroundTransfer**](/uwp/api/Windows.Networking.BackgroundTransfer) se devuelve como un valor **HRESULT**. El método [**BackgroundTransferError.GetStatus**](/uwp/api/windows.networking.backgroundtransfer.backgroundtransfererror.getstatus) se usa para convertir un error de red de una operación de transferencia en segundo plano en un valor de enumeración [**WebErrorStatus**](/uwp/api/Windows.Web.WebErrorStatus). La mayoría de los valores de enumeración **WebErrorStatus** corresponden a un error devuelto por la operación de cliente HTTP o FTP nativa. Una aplicación puede filtrar según valores **WebErrorStatus** concretos para modificar el comportamiento de la aplicación en función del motivo de la excepción.

Para los errores de validación de parámetros, una aplicación también puede usar el **HRESULT** de la excepción para obtener información más detallada del error que causó la excepción. Los valores posibles de **HRESULT** se enumeran en el archivo de encabezado *Winerror.h*. Para la mayoría de los errores de validación de parámetros, el valor de **HRESULT** devuelto es **E\_INVALIDARG**.

## <a name="important-apis"></a>API importantes
* [**Windows.Networking.BackgroundTransfer**](/uwp/api/windows.networking.backgroundtransfer)
* [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri)
* [**Windows.Networking.Sockets**](/uwp/api/Windows.Networking.Sockets)
