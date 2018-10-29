---
author: drewbatgit
ms.assetid: ''
description: Muestra cómo grabar audio para juegos, vídeo y los correspondientes metadatos en aplicaciones para UWP.
title: Capturar audio para juegos, vídeo, capturas de pantalla y metadatos
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, juego, capturar, audio, vídeo, metadatos
ms.localizationpriority: medium
ms.openlocfilehash: 6c1641cb4c50b86d7f678bf18fa85ad0215b4b15
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "5750680"
---
# <a name="capture-game-audio-video-screenshots-and-metadata"></a>Capturar audio para juegos, vídeo, capturas de pantalla y metadatos
Este artículo describe cómo capturar vídeo, audio y capturas de pantalla de juegos, y cómo enviar los metadatos que el sistema incrustará en medios capturados y difundidos, permitiendo que tus aplicaciones y otras puedan crear experiencias dinámicas que se sincronizan con eventos de los juegos. 

Hay dos maneras diferentes en que un juego se puede capturar en una aplicación para UWP. El usuario puede iniciar la captura con la interfaz de usuario del sistema integrada. Los elementos multimedia que se capturan con esta técnica se introducen en el ecosistema de juegos de Microsoft, pueden verse y compartirse a través de experiencias propias, como la aplicación XBox, y no están disponible para tu aplicación ni para los usuarios. Las primeras secciones de este artículo muestran cómo habilitar y deshabilitar la captura de aplicaciones implementada por el sistema y cómo recibir notificaciones cuando se inicia o se detiene la captura de aplicaciones .

La otra forma para capturar multimedia es usar las API del espacio de nombres **[Windows.Media.AppRecording](https://docs.microsoft.com/uwp/api/windows.media.apprecording)**. Si la captura está habilitada en el dispositivo, la aplicación puede comenzar a capturar el juego y, a continuación, después de pasado algún tiempo, puedes detener la captura, en cuyo momento los elementos multimedia se escriben en un archivo. Si el usuario ha habilitado la captura histórica, también puedes grabar juego que ya haya ocurrido especificando una hora de inicio en el pasado y una duración de grabación. Ambas técnicas generan un archivo de vídeo al que puede acceder la aplicación y, según donde elijas guardar los archivos, el usuario. Las secciones centrales de este artículo te llevan por la implementación de estos escenarios.

El espacio de nombres **[Windows.Media.Capture](https://docs.microsoft.com/uwp/api/windows.media.capture)** proporciona API para crear metadatos que describen el juego que se está capturando o difundiendo. Esto puede incluir texto o valores numéricos, con una etiqueta de texto que identifique cada elemento de datos. Los metadatos pueden representar un "evento" que se produce en un momento único, como cuando el usuario finaliza una vuelta en un juego de carreras, o puede representar un "estado" que persiste durante un período de tiempo, como el mapa de juego actual en el que el usuario está jugando. Los metadatos se escriben en una memoria caché que el sistema asigna y administra para la aplicación. Los metadatos se incrustan en secuencias de difusión y archivos de vídeo capturados, incluidas la captura integrada del sistema o técnicas de captura de aplicaciones personalizadas. Las secciones finales de este artículo muestran cómo escribir metadatos de un juego.

> [!NOTE] 
> Dado que los metadatos de un juego pueden incrustarse en los archivos multimedia que se pueden compartir en la red, fuera del control del usuario, en los metadatos no debe incluirse información personal identificable ni otros datos potencialmente sensibles.


## <a name="enable-and-disable-system-app-capture"></a>Habilitar y deshabilitar la captura de aplicaciones del sistema
La captura de aplicaciones del sistema la inicia el usuario con la interfaz de usuario del sistema integrada. Los archivos se introducen en el ecosistema de juegos de Windows y no están disponibles para la aplicación ni para el usuario, excepto para experiencias propias, como la aplicación XBox. La aplicación puede deshabilitar y habilitar las capturas de aplicaciones iniciada por el sistema, lo que permite evitar que el usuario capture determinado contenido o juego. 

Para habilitar o deshabilitar la captura de aplicaciones del sistema, simplemente llama al método estático **[AppCapture.SetAllowedAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.setallowedasync)** y pasa **false** para deshabilitar la captura o **true** para habilitarla.

[!code-cpp[SetAppCaptureAllowed](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetSetAppCaptureAllowed)]


## <a name="receive-notifications-when-system-app-capture-starts-and-stops"></a>Recibir notificaciones cuando se inicia y se detiene la captura de aplicaciones del sistema
Para recibir una notificación cuando comienza o finaliza la captura de aplicaciones del sistema, primero obtén una instancia de la clase **[AppCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture)** llamando al método de fábrica **[GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.GetForCurrentView)**. A continuación, registra un controlador del evento **[CapturingChanged](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.CapturingChanged)**.

[!code-cpp[RegisterCapturingChanged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRegisterCapturingChanged)]

En el controlador para el evento **CapturingChanged**, puedes comprobar las propiedades **[IsCapturingAudio](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.IsCapturingAudio)** y **[IsCapturingVideo](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.IsCapturingVideo)** para determinar si se está capturando audio o vídeo, respectivamente. Puede interesarte actualizar la interfaz de usuario de la aplicación para indicar el estado actual de captura.

[!code-cpp[OnCapturingChanged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnCapturingChanged)]

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>Agregar las extensiones de escritorio de Windows para UWP a tu aplicación
Las API para la grabación de audio y vídeo y para capturar pantallas directamente desde la aplicación, que se encuentran en el espacio de nombres **[Windows.Media.AppRecording](https://docs.microsoft.com/uwp/api/windows.media.apprecording)** no se incluyen en el contrato de API universal. Para acceder a las API, debes agregar una referencia a las extensiones de escritorio de Windows para UWP a tu aplicación, con los siguientes pasos.

1. En Visual Studio, en **Explorador de soluciones**, expande el proyecto UWP, haz clic con el botón derecho en **Referencias** y entonces selecciona **Agregar referencia...**. 
2. Expande el nodo **Windows Universal** y selecciona **Extensiones**.
3. En la lista de extensiones, marca la casilla de verificación junto a la entrada de **Extensiones de escritorio de Windows para UWP** que coincida con la compilación de destino del proyecto. Para las funciones de difusión de aplicaciones, la versión debe ser 1709 o superior.
4. Haz clic en **Aceptar**.

## <a name="get-an-instance-of-apprecordingmanager"></a>Obtén una instancia de AppRecordingManager
La clase **[AppRecordingManager](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager)** es la API central que usarás para administrar la grabación de aplicaciones. Obtén una instancia de esta clase llamando al método de fábrica **[GetDefault](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.GetDefault)**. Antes de usar cualquiera de las API en el espacio de nombres **Windows.Media.AppRecording**, deberías comprobar su presencia en el dispositivo actual. Las API no están disponibles en dispositivos que ejecutan una versión de sistema operativo anterior a Windows 10, versión 1709. En lugar de comprobar si hay una versión específica del sistema operativo, usa el método **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** para consultar la *Windows.Media.AppBroadcasting.AppRecordingContract*, versión 1.0. Si este contrato está presente, las API de grabación están disponibles en el dispositivo. El código de ejemplo en este artículo comprueba las API una vez y, a continuación, comprueba si **AppRecordingManager** tiene valor null antes de operaciones posteriores.

[!code-cpp[GetAppRecordingManager](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetGetAppRecordingManager)]

## <a name="determine-if-your-app-can-currently-record"></a>Determinar si la aplicación puede grabar actualmente
Existen varios motivos por los que la aplicación tal vez no pueda capturar audio o vídeo actualmente, incluyendo si el dispositivo actual no cumple los requisitos de hardware para la grabación o si otra aplicación está actualmente difundiendo. Antes de iniciar una grabación, puedes comprobar si la aplicación es capaz de grabar actualmente. Llama al método **[GetStatus](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** del objeto **AppRecordingManager** y, a continuación, comprueba la propiedad **[CanRecord](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecord)** del objeto devuelto **[AppRecordingStatus](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus)**. Si **CanRecord** devuelve **false**, lo que significa que la aplicación no puede grabar actualmente, puedes comprobar la propiedad **[Detalles](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.Details)** para determinar el motivo. Según el motivo, puede que te interese mostrar el estado al usuario o mostrar las instrucciones para habilitar la grabación de la aplicación.



[!code-cpp[CanRecord](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCanRecord)]

## <a name="manually-start-and-stop-recording-your-app-to-a-file"></a>Iniciar y detener manualmente la grabación de la aplicación en un archivo

Después de comprobar que tu aplicación sea capaz de grabar, puedes iniciar una nueva grabación, llamando al método **[StartRecordingToFileAsync](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.startrecordingtofileasync)** del objeto **AppRecordingManager**.

En el siguiente ejemplo, el primer bloque **then** se ejecuta cuando se produce un error en la tarea asincrónica. El segundo bloque **then** intenta acceder al resultado de la tarea y, si el resultado es null, en ese caso la tarea se ha completado. En ambos casos, el método auxiliar **OnRecordingComplete**, mostrado a continuación, se llama para manejar el resultado. 

[!code-cpp[StartRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartRecordToFile)]

Cuando se completa la operación de grabación, comprueba la propiedad **[Succeeded](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** del objeto devuelto **[AppRecordingResult](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult)** para determinar si la operación de registro se realizó correctamente. Si es así, puedes comprobar la propiedad **[IsFileTruncated](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** para determinar si, por motivos de almacenamiento, el sistema se vio obligado a truncar el archivo capturado. Puedes comprobar la propiedad **[Duration](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** para descubrir la duración real del archivo grabado que, si el archivo se trunca, puede ser menor que la duración de la operación de grabación.

[!code-cpp[OnRecordingComplete](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnRecordingComplete)]

Los siguientes ejemplos muestran algo de código básico para iniciar y detener la operación de grabación que se muestra en el ejemplo anterior.

[!code-cpp[CallStartRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallStartRecordToFile)]

[!code-cpp[FinishRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetFinishRecordToFile)]

## <a name="record-a-historical-time-span-to-a-file"></a>Grabar un intervalo de tiempo histórico en un archivo
Si el usuario ha habilitado la grabación histórica de la aplicación en la configuración del sistema, puede grabar un intervalo de tiempo del juego que ha transcurrido anteriormente. Un ejemplo anterior de este artículo mostró cómo confirmar si tu aplicación puede grabar actualmente juego. Hay una comprobación adicional para determinar si la captura histórica está habilitada. Una vez más, llama a **[GetStatus](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** y comprueba la propiedad **[CanRecordTimeSpan](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecordTimeSpan)** del objeto devuelto **AppRecordingStatus**. Este ejemplo también se devuelve la propiedad **[HistoricalBufferDuration](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.HistoricalBufferDuration)** de **AppRecordingStatus**, que se usará para determinar una hora de inicio válida para la operación de grabación.

[!code-cpp[CanRecordTimeSpan](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCanRecordTimeSpan)]

Para capturar un intervalo de tiempo histórico, debes especificar una hora de inicio de la grabación y una duración. La hora de inicio se proporciona como una estructura **[DateTime](https://docs.microsoft.com/uwp/api/windows.foundation.datetime)**. La hora de inicio debe ser una hora anterior a la hora actual, dentro de la longitud del búfer de grabación histórica. Para este ejemplo, se recupera la longitud del búfer como parte de la comprobación para ver si está habilitada la grabación histórica, lo que se muestra en el ejemplo de código anterior. La duración de la grabación histórica se proporciona como una estructura **[TimeSpan](https://docs.microsoft.com/uwp/api/windows.foundation.timespan)**, que también debe ser igual o más pequeña que la duración del búfer histórico. Una vez que hayas determinado la hora de inicio y la duración deseadas, llama a **[RecordTimeSpanToFileAsync](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.recordtimespantofileasync)** para iniciar la operación de grabación.

Como en la grabación con inicio y parada manual, cuando se completa una grabación histórica, puedes comprobar la propiedad **[Succeeded](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** del objeto **[AppRecordingResult](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult)** devuelto, para determinar si la operación de registro se realizó correctamente, y puedes comprobar las propiedades **[IsFileTruncated](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** y **[Duration](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** para descubrir la duración real del archivo grabado que, si el archivo se trunca, puede ser menor que la duración de la ventana de tiempo solicitada.

[!code-cpp[RecordTimeSpanToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRecordTimeSpanToFile)]

Los siguientes ejemplos muestran algo de código básico para iniciar la operación de grabación que se muestra en el ejemplo anterior.

[!code-cpp[CallRecordTimeSpanToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallRecordTimeSpanToFile)]

## <a name="save-screenshot-images-to-files"></a>Guardar imágenes de captura de pantalla en archivos
La aplicación puede iniciar una captura de pantalla que guardará el contenido actual de la ventana de la aplicación en un archivo de imagen o en varios archivos de imagen con diferentes codificaciones de imagen. Para especificar las codificaciones de imagen que quieres usar, crea una lista de cadenas, donde cada una de ellas representa un tipo de imagen. Las propiedades de **[ImageEncodingSubtypes](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingsubtypes)** proporcionan la cadena correcta para cada tipo de imagen compatible, como **MediaEncodingSubtypes.Png** o **MediaEncodingSubtypes.JpegXr**.

Inicia la captura de pantalla mediante una llamada al método **[SaveScreenshotToFilesAsync](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.savescreenshottofilesasync)** del objeto **AppRecordingManager**. El primer parámetro de este método es un **StorageFolder** donde se guardarán los archivos de imagen. El segundo parámetro es un prefijo de nombre de archivo al que el sistema agregará la extensión para cada tipo de imagen guardado, como ".png".

El tercer parámetro de **SaveScreenshotToFilesAsync** es necesario para que el sistema pueda realizar la conversión de espacio de color adecuada si la ventana actual a capturar está mostrando contenido HDR. Si hay contenido HDR, este parámetro debe establecerse en **AppRecordingSaveScreenshotOption.HdrContentVisible**. De lo contrario, usa **AppRecordingSaveScreenshotOption.None**. El último parámetro del método es la lista de formatos de imagen en que se debe capturar la pantalla.

Cuando la llamada asíncrona a **SaveScreenshotToFilesAsync** finaliza, devuelve un objeto **[AppRecordingSavedScreenshotInfo](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingsavedscreenshotinfo)** que ofrece el valor **StorageFile** y los **MediaEncodingSubtypes** asociados, que indican el tipo de imagen de cada imagen guardada.

[!code-cpp[SaveScreenShotToFiles](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetSaveScreenShotToFiles)]

Los siguientes ejemplos muestran algo de código básico para iniciar la operación de captura de pantalla que se muestra en el ejemplo anterior.

[!code-cpp[CallSaveScreenShotToFiles](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallSaveScreenShotToFiles)]

## <a name="add-game-metadata-for-system-and-app-initiated-capture"></a>Agregar metadatos de juego para capturas del sistema e iniciadas por aplicaciones
Las siguientes secciones de este artículo describen cómo proporcionar metadatos que el sistema insertará en la secuencia de MP4 de juego capturado o difundido. Pueden incrustarse metadatos en los medios que se capturan usando la interfaz de usuario integrada del sistema y medios capturados por la aplicación con **AppRecordingManager**. Estos metadatos puede extraerlos la aplicación y otras aplicaciones durante la reproducción multimedia, con el fin de proporcionar experiencias dependientes del contexto que se sincronizan con el juego capturado o difundido.

### <a name="get-an-instance-of-appcapturemetadatawriter"></a>Obtener una instancia de AppCaptureMetadataWriter
La clase principal para administrar metadatos de captura de aplicaciones es **[AppCaptureMetadataWriter](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter)**. Antes de inicializar una instancia de esta clase, usa el método **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** para consultar *Windows.Media.Capture.AppCaptureMetadataContract*, versión 1.0, para comprobar que la API está disponible en el dispositivo actual.

[!code-cpp[GetMetadataWriter](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetGetMetadataWriter)]

### <a name="write-metadata-to-the-system-cache-for-your-app"></a>Escribir metadatos en la memoria caché del sistema para tu aplicación
Cada elemento de metadatos tiene una etiqueta de cadena que identifica el elemento de metadatos, un valor de datos asociado que puede ser una cadena, un entero o un valor doble y un valor de la enumeración **[AppCaptureMetadataPriority](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatapriority)**, que indica la prioridad relativa del elemento de datos. Un elemento de metadatos puede bien considerarse un "evento", que se produce en un único punto del tiempo, o un "estado", que mantiene un valor a lo largo de una ventana de tiempo. Los metadatos se escriben en una memoria caché que el sistema asigna y administra para la aplicación. El sistema impone un límite de tamaño en la memoria caché de metadatos y, cuando se alcanza el límite, purga datos en función de la prioridad con que cada elemento los metadatos se haya escrito. La siguiente sección de este artículo muestra cómo administrar la asignación de memoria de metadatos de la aplicación.

Una aplicación típica puede optar por escribir algunos metadatos al principio de la sesión de captura para proporcionar contexto para los datos subsiguientes. Para este escenario se recomienda que uses datos de "evento" instantáneos. Este ejemplo llama a **[AddStringEvent](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.addstringevent)**, **[AddDoubleEvent](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.adddoubleevent)**, y **[AddInt32Event](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.addint32event)** para establecer los valores instantáneos para cada tipo de datos.

[!code-cpp[StartSession](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartSession)]

Un escenario común para el uso de datos de "estado" que persisten con el tiempo es realizar el mapa de juego en el que el jugador se encuentra actualmente. Este ejemplo llama a **[StartStringState](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.startstringstate)** para establecer el valor del estado. 

[!code-cpp[StartMap](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartMap)]

Llama a **[StopState](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.stopstate)** para registrar que un estado concreto ha finalizado.

[!code-cpp[EndMap](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetEndMap)]

Puedes sobrescribir un estado estableciendo un valor nuevo con una etiqueta de estado existente.

[!code-cpp[LevelUp](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetLevelUp)]

Puedes finalizar todos los estados abiertos llamando a **[StopAllStates](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.StopAllStates)**.

[!code-cpp[RaceComplete](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRaceComplete)]

### <a name="manage-metadata-cache-storage-limit"></a>Administrar el límite de almacenamiento de caché de metadatos
Los metadatos que escribes con **AppCaptureMetadataWriter** se almacenan en caché por parte del sistema hasta que se escriben en la emisión multimedia asociada. El sistema define un límite de tamaño de la caché de metadatos de cada aplicación. Una vez alcanzado el límite de tamaño de caché, el sistema empezará a purgar los metadatos almacenados en caché. El sistema eliminará los metadatos escritos con valor de prioridad **[AppCaptureMetadataPriority.Informational](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatapriority)** antes de eliminar los metadatos con prioridad **[AppCaptureMetadataPriority.Important](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatapriority)**.

En cualquier momento, puedes comprobar para ver el número de bytes disponibles en la caché de metadatos de la aplicación mediante una llamada a **[RemainingStorageBytesAvailable](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.RemainingStorageBytesAvailable)**. Puedes establecer tu propio umbral definido por la aplicación, tras lo cual puedes decidir reducir la cantidad de metadatos que escribes en la memoria caché. El siguiente ejemplo muestra una sencilla implementación de este patrón.

[!code-cpp[CheckMetadataStorage](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCheckMetadataStorage)]

[!code-cpp[ComboExecuted](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetComboExecuted)]

### <a name="receive-notifications-when-the-system-purges-metadata"></a>Recibir notificaciones cuando el sistema depura metadatos
Puedes registrar para recibir una notificación cuando el sistema comienza la depuración de metadatos de la aplicación, registrando un controlador para el evento **[MetadataPurged](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.MetadataPurged)**.

[!code-cpp[RegisterMetadataPurged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRegisterMetadataPurged)]

En el controlador del evento **MetadataPurged**, puedes liberar algún espacio en la memoria caché de metadatos terminando los estados de prioridad, puedes implementar lógica definida por la aplicación para reducir la cantidad de metadatos que escribes en la caché o no hacer nada y dejar que el sistema continúe para purgar la memoria caché en función de la prioridad con la que se escribió.

[!code-cpp[OnMetadataPurged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnMetadataPurged)]

## <a name="related-topics"></a>Temas relacionados

* [Juegos](index.md)
 

 




