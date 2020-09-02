---
ms.assetid: ''
description: Obtenga información acerca de cómo capturar vídeo, audio y capturas de pantalla, y cómo enviar metadatos que el sistema inserta en medios capturados y de difusión.
title: Capturar audio para juegos, vídeo, capturas de pantalla y metadatos
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, juego, captura, audio, vídeo, metadatos
ms.localizationpriority: medium
ms.openlocfilehash: ea01139d4945d1e1b7e9a49078cd93725388e946
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364148"
---
# <a name="capture-game-audio-video-screenshots-and-metadata"></a>Capturar audio para juegos, vídeo, capturas de pantalla y metadatos
En este artículo se describe cómo capturar vídeo, audio y capturas de pantalla, y cómo enviar metadatos que el sistema insertará en medios capturados y de difusión, lo que permite a la aplicación y a otros crear experiencias dinámicas sincronizadas con eventos de juego. 

Hay dos maneras diferentes de capturar el juego en una aplicación de UWP. El usuario puede iniciar la captura con la interfaz de usuario integrada del sistema. Los medios que se capturan con esta técnica se ingestan en el ecosistema de juegos de Microsoft, se pueden ver y compartir a través de experiencias de primera parte, como la aplicación Xbox, y no están directamente disponibles para la aplicación o para los usuarios. En las primeras secciones de este artículo se muestra cómo habilitar y deshabilitar la captura de aplicaciones implementadas por el sistema y cómo recibir notificaciones cuando se inicia o se detiene la captura de la aplicación.

La otra forma de capturar los medios es usar las API del espacio de nombres **[Windows. Media. AppRecording](/uwp/api/windows.media.apprecording)** . Si la captura está habilitada en el dispositivo, la aplicación puede iniciar la captura del juego y, después de que haya transcurrido un tiempo, puede detener la captura, momento en el que el medio se escribe en un archivo. Si el usuario ha habilitado la captura histórica, también puede grabar el juego que ya se ha producido especificando una hora de inicio en el pasado y una duración para registrar. Estas dos técnicas generan un archivo de vídeo al que puede acceder la aplicación y, en función de dónde decida guardar los archivos, el usuario. Las secciones centrales de este artículo le guiarán a través de la implementación de estos escenarios.

El espacio de nombres **[Windows. Media. Capture](/uwp/api/windows.media.capture)** proporciona las API para crear metadatos que describen el juego que se va a capturar o difundir. Esto puede incluir valores de texto o numéricos, con una etiqueta de texto que identifica cada elemento de datos. Los metadatos pueden representar un "evento" que tiene lugar en un momento dado, como cuando el usuario finaliza una vuelta en un juego de carreras, o puede representar un "estado" que se mantiene en un intervalo de tiempo, como la asignación de juego actual en la que el usuario está jugando. Los metadatos se escriben en una memoria caché que el sistema asigna y administra para la aplicación. Los metadatos se insertan en secuencias de difusión y archivos de vídeo capturados, incluidas las técnicas de captura de sistema integrada o de captura de aplicaciones personalizadas. En las secciones finales de este artículo se muestra cómo escribir metadatos de juegos.

> [!NOTE] 
> Dado que los metadatos de juego se pueden insertar en archivos multimedia que se pueden compartir a través de la red, fuera del control del usuario, no debe incluir información de identificación personal u otros datos potencialmente confidenciales en los metadatos.


## <a name="enable-and-disable-system-app-capture"></a>Habilitar y deshabilitar la captura de aplicaciones del sistema
El usuario inicia la captura de la aplicación del sistema con la interfaz de usuario integrada del sistema. Los archivos son ingeridos por el ecosistema de juegos de Windows y no están disponibles para la aplicación o el usuario, excepto a través de experiencias de primera entidad como la aplicación Xbox. La aplicación puede deshabilitar y habilitar la captura de aplicaciones iniciadas por el sistema, lo que le permite impedir que el usuario Capture cierto contenido o juego. 

Para habilitar o deshabilitar la captura de una aplicación del sistema, basta con llamar al método estático **[AppCapture. SetAllowedAsync](/uwp/api/windows.media.capture.appcapture.setallowedasync)** y pasar **false** para deshabilitar la captura o **true** para habilitar la captura.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetSetAppCaptureAllowed":::


## <a name="receive-notifications-when-system-app-capture-starts-and-stops"></a>Recepción de notificaciones cuando se inicia y se detiene la captura de aplicaciones del sistema
Para recibir una notificación cuando comienza o finaliza la captura de la aplicación del sistema, primero obtenga una instancia de la clase **[AppCapture](/uwp/api/windows.media.capture.appcapture)** llamando al Factory Method **[GetForCurrentView](/uwp/api/windows.media.capture.appcapture.GetForCurrentView)**. A continuación, registre un controlador para el evento **[CapturingChanged](/uwp/api/windows.media.capture.appcapture.CapturingChanged)** .

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetRegisterCapturingChanged":::

En el controlador del evento **CapturingChanged** , puede comprobar las propiedades **[IsCapturingAudio](/uwp/api/windows.media.capture.appcapture.IsCapturingAudio)** y **[IsCapturingVideo](/uwp/api/windows.media.capture.appcapture.IsCapturingVideo)** para determinar si el audio o el vídeo se capturan respectivamente. Puede que desee actualizar la interfaz de usuario de la aplicación para indicar el estado de captura actual.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetOnCapturingChanged":::

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>Agregar las extensiones de escritorio de Windows para UWP a la aplicación
Las API para grabar audio y vídeo y para capturar capturas de pantalla directamente desde su aplicación, que se encuentran en el espacio de nombres **[Windows. Media. AppRecording](/uwp/api/windows.media.apprecording)** , no se incluyen en el contrato de la API universal. Para acceder a las API, debe agregar una referencia a las extensiones de escritorio de Windows para UWP a la aplicación con los pasos siguientes.

1. En el **Explorador de soluciones**de Visual Studio, expanda el proyecto de UWP y haga clic con el botón derecho en **referencias** y seleccione **Agregar referencia..**.. 
2. Expanda el nodo **universal de Windows** y seleccione **extensiones**.
3. En la lista de extensiones, active la casilla situada junto a **extensiones de escritorio de Windows para la** entrada de UWP que coincida con la compilación de destino del proyecto. En el caso de las características de difusión de aplicaciones, la versión debe ser 1709 o superior.
4. Haga clic en **OK**.

## <a name="get-an-instance-of-apprecordingmanager"></a>Obtener una instancia de AppRecordingManager
La clase **[AppRecordingManager](/uwp/api/windows.media.apprecording.apprecordingmanager)** es la API central que usará para administrar la grabación de la aplicación. Obtenga una instancia de esta clase llamando a la Factory Method **[GetDefault](/uwp/api/windows.media.apprecording.apprecordingmanager.GetDefault)**. Antes de usar cualquiera de las API en el espacio de nombres **Windows. Media. AppRecording** , debe comprobar su presencia en el dispositivo actual. Las API no están disponibles en los dispositivos que ejecutan una versión de SO anterior a la versión 1709 de Windows 10. En lugar de buscar una versión específica del sistema operativo, use el método **[ApiInformation. IsApiContractPresent](/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** para consultar la versión 1,0 de *Windows. Media. AppBroadcasting. AppRecordingContract* . Si este contrato está presente, las API de grabación están disponibles en el dispositivo. En el código de ejemplo de este artículo se comprueban las API una vez y después se comprueba si el valor de **AppRecordingManager** es NULL antes de las operaciones posteriores.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetGetAppRecordingManager":::

## <a name="determine-if-your-app-can-currently-record"></a>Determinar si la aplicación puede registrar actualmente
Hay varios motivos por los que es posible que la aplicación no pueda capturar audio o vídeo, lo que incluye si el dispositivo actual no cumple los requisitos de hardware para la grabación o si otra aplicación está difundiendo en este momento. Antes de iniciar una grabación, puede comprobar si la aplicación puede grabarse actualmente. Llame al método **[getStatus](/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** del objeto **AppRecordingManager** y, a continuación, Compruebe la propiedad **[CanRecord](/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecord)** del objeto **[AppRecordingStatus](/uwp/api/windows.media.apprecording.apprecordingstatus)** devuelto. Si **CanRecord** devuelve **false**, lo que significa que la aplicación no se puede registrar actualmente, puede comprobar la propiedad **[Details](/uwp/api/windows.media.apprecording.apprecordingstatus.Details)** para determinar el motivo. En función de la razón, puede que desee mostrar el estado al usuario o mostrar instrucciones para habilitar la grabación de la aplicación.



:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCanRecord":::

## <a name="manually-start-and-stop-recording-your-app-to-a-file"></a>Iniciar y detener manualmente la grabación de la aplicación en un archivo

Después de comprobar que la aplicación pueda grabar, puede iniciar una nueva grabación llamando al método **[StartRecordingToFileAsync](/uwp/api/windows.media.apprecording.apprecordingmanager.startrecordingtofileasync)** del objeto **AppRecordingManager** .

En el ejemplo **siguiente, el primer bloque** se ejecuta cuando se produce un error en la tarea asincrónica. En segundo lugar **,** se bloquean los intentos de obtener acceso al resultado de la tarea y, si el resultado es null, la tarea se ha completado. En ambos casos, se llama al método auxiliar **OnRecordingComplete** , que se muestra a continuación, para controlar el resultado. 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetStartRecordToFile":::

Cuando se complete la operación de grabación, Compruebe la propiedad **[Succeeded](/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** del objeto **[AppRecordingResult](/uwp/api/windows.media.apprecording.apprecordingresult)** devuelto para determinar si la operación de registro se realizó correctamente. Si es así, puede comprobar la propiedad **[IsFileTruncated](/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** para determinar si, por motivos de almacenamiento, el sistema se forzó para truncar el archivo capturado. Puede comprobar la propiedad **[Duration](/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** para detectar la duración real del archivo grabado que, si el archivo está truncado, puede ser menor que la duración de la operación de grabación.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetOnRecordingComplete":::

En los siguientes ejemplos se muestra código básico para iniciar y detener la operación de grabación que se muestra en el ejemplo anterior.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCallStartRecordToFile":::

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetFinishRecordToFile":::

## <a name="record-a-historical-time-span-to-a-file"></a>Registrar un intervalo de tiempo histórico en un archivo
Si el usuario ha habilitado la grabación histórica de la aplicación en la configuración del sistema, puede registrar un intervalo de tiempo de juego que ya ha transcurrido. En un ejemplo anterior de este artículo se ha mostrado cómo confirmar que la aplicación puede grabar el juego actualmente. Hay una comprobación adicional para determinar si la captura histórica está habilitada. Una vez más, llame a **[getStatus](/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** y Compruebe la propiedad **[CanRecordTimeSpan](/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecordTimeSpan)** del objeto **AppRecordingStatus** devuelto. En este ejemplo también se devuelve la propiedad **[HistoricalBufferDuration](/uwp/api/windows.media.apprecording.apprecordingstatus.HistoricalBufferDuration)** de **AppRecordingStatus** que se usará para determinar una hora de inicio válida para la operación de grabación.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCanRecordTimeSpan":::

Para capturar un intervalo de tiempo histórico, debe especificar una hora de inicio para la grabación y una duración. La hora de inicio se proporciona como una estructura **[DateTime](/uwp/api/windows.foundation.datetime)** . La hora de inicio debe ser una hora anterior a la hora actual, dentro de la longitud del búfer de grabación histórico. En este ejemplo, la longitud del búfer se recupera como parte de la comprobación para ver si la grabación histórica está habilitada, que se muestra en el ejemplo de código anterior. La duración de la grabación histórica se proporciona como un struct  **[TimeSpan](/uwp/api/windows.foundation.timespan)** , que también debe ser igual o menor que la duración del búfer histórico. Una vez que haya determinado la hora de inicio y la duración deseadas, llame a **[RecordTimeSpanToFileAsync](/uwp/api/windows.media.apprecording.apprecordingmanager.recordtimespantofileasync)** para iniciar la operación de grabación.

Al igual que la grabación con inicio y detención manual, cuando se completa una grabación histórica, puede comprobar la propiedad **[Succeeded](/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** del objeto **[AppRecordingResult](/uwp/api/windows.media.apprecording.apprecordingresult)** devuelto para determinar si la operación de registro se realizó correctamente, y puede comprobar la propiedad **[IsFileTruncated](/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** y **[Duration](/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** para detectar la duración real del archivo grabado que, si el archivo está truncado, puede ser menor que la duración de la ventana de tiempo solicitada.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetRecordTimeSpanToFile":::

En el ejemplo siguiente se muestra código básico para iniciar la operación de registro histórico que se muestra en el ejemplo anterior.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCallRecordTimeSpanToFile":::

## <a name="save-screenshot-images-to-files"></a>Guardar imágenes de captura de pantalla en archivos
La aplicación puede iniciar una captura de captura de pantalla que guardará el contenido actual de la ventana de la aplicación en un archivo de imagen o en varios archivos de imagen con diferentes codificaciones de imagen. Para especificar las codificaciones de imagen que desea usar, cree una lista de cadenas en las que cada una representa un tipo de imagen. Las propiedades de **[ImageEncodingSubtypes](/uwp/api/windows.media.mediaproperties.mediaencodingsubtypes)** proporcionan la cadena correcta para cada tipo de imagen compatible, como **MediaEncodingSubtypes.Png** o **MediaEncodingSubtypes. JpegXr**.

Inicie la captura de pantalla mediante una llamada al método **[SaveScreenshotToFilesAsync](/uwp/api/windows.media.apprecording.apprecordingmanager.savescreenshottofilesasync)** del objeto **AppRecordingManager** . El primer parámetro de este método es un **StorageFolder** en el que se guardarán los archivos de imagen. El segundo parámetro es un prefijo de nombre de archivo en el que el sistema anexará la extensión para cada tipo de imagen guardado, como ". png".

El tercer parámetro de **SaveScreenshotToFilesAsync** es necesario para que el sistema pueda realizar la conversión ColorSpace adecuada si la ventana actual que se va a capturar muestra el contenido de HDR. Si el contenido de HDR está presente, este parámetro debe establecerse en **AppRecordingSaveScreenshotOption. HdrContentVisible**. De lo contrario, use **AppRecordingSaveScreenshotOption. None**. El último parámetro del método es la lista de formatos de imagen en la que se debe capturar la pantalla.

Cuando se completa la llamada asincrónica a **SaveScreenshotToFilesAsync** , devuelve un objeto **[AppRecordingSavedScreenshotInfo](/uwp/api/windows.media.apprecording.apprecordingsavedscreenshotinfo)** que proporciona el **StorageFile** y el valor de **MediaEncodingSubtypes** asociado que indica el tipo de imagen de cada imagen guardada.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetSaveScreenShotToFiles":::

En el ejemplo siguiente se muestra código básico para iniciar la operación de captura de pantalla que se muestra en el ejemplo anterior.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCallSaveScreenShotToFiles":::

## <a name="add-game-metadata-for-system-and-app-initiated-capture"></a>Agregar metadatos de juego para la captura iniciada por el sistema y por la aplicación
En las siguientes secciones de este artículo se describe cómo proporcionar los metadatos que el sistema insertará en el flujo MP4 del juego capturado o de difusión. Los metadatos se pueden insertar en medios que se capturan mediante la interfaz de usuario integrada del sistema y los medios capturados por la aplicación con **AppRecordingManager**. Estos metadatos pueden ser extraídos por la aplicación y otras aplicaciones durante la reproducción multimedia para proporcionar experiencias contextuales que se sincronizan con el juego capturado o de difusión.

### <a name="get-an-instance-of-appcapturemetadatawriter"></a>Obtener una instancia de AppCaptureMetadataWriter
La clase principal para administrar los metadatos de captura de la aplicación es **[AppCaptureMetadataWriter](/uwp/api/windows.media.capture.appcapturemetadatawriter)**. Antes de inicializar una instancia de esta clase, use el método **[ApiInformation. IsApiContractPresent](/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** para consultar la versión 1,0 de *Windows. Media. Capture. AppCaptureMetadataContract* para comprobar que la API está disponible en el dispositivo actual.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetGetMetadataWriter":::

### <a name="write-metadata-to-the-system-cache-for-your-app"></a>Escritura de metadatos en la memoria caché del sistema para la aplicación
Cada elemento de metadatos tiene una etiqueta de cadena que identifica el elemento de metadatos, un valor de datos asociado que puede ser una cadena, un entero o un valor doble, y un valor de la enumeración **[AppCaptureMetadataPriority](/uwp/api/windows.media.capture.appcapturemetadatapriority)** que indica la prioridad relativa del elemento de datos. Un elemento de metadatos se puede considerar un "evento", que se produce en un único punto en el tiempo o un "estado" que mantiene un valor en una ventana de tiempo. Los metadatos se escriben en una memoria caché asignada y administrada por el sistema para la aplicación. El sistema aplica un límite de tamaño en la memoria caché de la memoria de metadatos y, cuando se alcanza el límite, purga los datos en función de la prioridad con la que se escribió cada elemento de metadatos. En la siguiente sección de este artículo se muestra cómo administrar la asignación de memoria de metadatos de la aplicación.

Una aplicación típica puede optar por escribir algunos metadatos al principio de la sesión de captura para proporcionar algún contexto para los datos siguientes. En este escenario, se recomienda usar datos de "eventos" instantáneos. En este ejemplo se llama a **[AddStringEvent](/uwp/api/windows.media.capture.appcapturemetadatawriter.addstringevent)**, **[AddDoubleEvent](/uwp/api/windows.media.capture.appcapturemetadatawriter.adddoubleevent)** y **[AddInt32Event](/uwp/api/windows.media.capture.appcapturemetadatawriter.addint32event)** para establecer valores instantáneos para cada tipo de datos.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetStartSession":::

Un escenario común para usar datos de "estado" que se conservan con el tiempo es realizar un seguimiento de la asignación de juego en la que está actualmente el reproductor. En este ejemplo se llama a **[StartStringState](/uwp/api/windows.media.capture.appcapturemetadatawriter.startstringstate)** para establecer el valor de estado. 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetStartMap":::

Llame a **[StopState](/uwp/api/windows.media.capture.appcapturemetadatawriter.stopstate)** para registrar que un estado determinado ha finalizado.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetEndMap":::

Puede sobrescribir un estado estableciendo un nuevo valor con una etiqueta de estado existente.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetLevelUp":::

Puede finalizar todos los Estados abiertos actualmente llamando a **[StopAllStates](/uwp/api/windows.media.capture.appcapturemetadatawriter.StopAllStates)**.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetRaceComplete":::

### <a name="manage-metadata-cache-storage-limit"></a>Administrar límite de almacenamiento en caché de metadatos
Los metadatos que se escriben con **AppCaptureMetadataWriter** se almacenan en caché por el sistema hasta que se escriben en la secuencia multimedia asociada. El sistema define un límite de tamaño para la caché de metadatos de cada aplicación. Una vez alcanzado el límite de tamaño de la caché, el sistema comenzará a purgar los metadatos almacenados en caché. El sistema eliminará los metadatos que se escribieron con AppCaptureMetadataPriority. valor de prioridad **[informativo](/uwp/api/windows.media.capture.appcapturemetadatapriority)** antes de eliminar metadatos con la prioridad **[AppCaptureMetadataPriority. important](/uwp/api/windows.media.capture.appcapturemetadatapriority)** .

En cualquier momento, puede comprobar para ver el número de bytes disponibles en la memoria caché de metadatos de la aplicación mediante una llamada a **[RemainingStorageBytesAvailable](/uwp/api/windows.media.capture.appcapturemetadatawriter.RemainingStorageBytesAvailable)**. Puede elegir establecer su propio umbral definido por la aplicación después del cual puede optar por reducir la cantidad de metadatos que se escriben en la memoria caché. En el ejemplo siguiente se muestra una implementación simple de este patrón.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCheckMetadataStorage":::

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetComboExecuted":::

### <a name="receive-notifications-when-the-system-purges-metadata"></a>Recibir notificaciones cuando el sistema purga los metadatos
Puede registrarse para recibir una notificación cuando el sistema empiece a purgar los metadatos de la aplicación mediante el registro de un controlador para el evento **[MetadataPurged](/uwp/api/windows.media.capture.appcapturemetadatawriter.MetadataPurged)** .

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetRegisterMetadataPurged":::

En el controlador del evento **MetadataPurged** , puede borrar algún espacio en la caché de metadatos al finalizar los Estados de prioridad más baja, puede implementar la lógica definida por la aplicación para reducir la cantidad de metadatos que se escriben en la memoria caché, o bien no hacer nada y dejar que el sistema Siga purgando la memoria caché en función de la prioridad con la que se escribió.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetOnMetadataPurged":::

## <a name="related-topics"></a>Temas relacionados

* [Juegos](index.md)
 

 
