---
ms.assetid: 0186EA01-8446-45BA-A109-C5EB4B80F368
description: En este artículo se muestra cómo usar la clase AdvancedPhotoCapture para capturar fotos con poca luz y alto rango dinámico (HDR).
title: Captura de fotos con poca luz y alto rango dinámico (HDR)
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dde7f966e47aa6c35e3bc4e508eddabf13e313ee
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598950"
---
# <a name="high-dynamic-range-hdr-and-low-light-photo-capture"></a>Captura de fotos con poca luz y alto rango dinámico (HDR)



En este artículo se muestra cómo usar la clase [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) para capturar fotos de alto rango dinámico (HDR). Esta API también te permite obtener un marco de referencia de la captura HDR antes de que finalice el procesamiento de la imagen final.

Otros artículos relacionados con la captura HDR incluyen:

-   Puedes usar la clase [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/dn948902) para permitir que el sistema evalúe el contenido del flujo de vista previa de la captura multimedia para determinar si el procesamiento HDR mejoraría el resultado de la captura. Para obtener más información, consulta [Análisis de la escena para la captura multimedia](scene-analysis-for-media-capture.md).

-   Usa la clase [**HdrVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926680) para capturar vídeo con el algoritmo de procesamiento HDR integrado de Windows. Para obtener más información, consulta [Controles de dispositivo de captura para captura de vídeos](capture-device-controls-for-video-capture.md).

-   Puedes usar la clase [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564) para capturar una secuencia de fotos, cada una con configuraciones de captura diferentes, e implementar un tipo de HDR propio u otro algoritmo de procesamiento. Para obtener más información, consulta [Secuencia de fotos variable](variable-photo-sequence.md).



> [!NOTE] 
> A partir de Windows 10, versión 1709, se admite la grabación de vídeo y el uso de **AdvancedPhotoCapture** de manera simultánea.  Esto no se admite en versiones anteriores. Este cambio significa que puedes tener un **[LowLagMediaRecording](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording)** y **[AdvancedPhotoCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.advancedphotocapture)** preparados al mismo tiempo. Puedes iniciar o detener la grabación de vídeo entre llamadas en **[MediaCapture.PrepareAdvancedPhotoCaptureAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.prepareadvancedphotocaptureasync)** y **[AdvancedPhotoCapture.FinishAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.advancedphotocapture.FinishAsync)**. También puedes llamar a **[AdvancedPhotoCapture.CaptureAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.advancedphotocapture.CaptureAsync)** mientras el vídeo se está grabando. Sin embargo, algunos escenarios de **AdvancedPhotoCapture**, como la captura de una foto HDR mientras se graba vídeo provocaría la alteración de algunos fotogramas de vídeo por la captura HDR, lo que provocaría una experiencia negativa del usuario. Por este motivo, la lista de modos devueltos por **[AdvancedPhotoControl.SupportedModes](https://docs.microsoft.com/uwp/api/windows.media.devices.advancedphotocontrol.SupportedModes)** será diferente mientras se está grabando vídeo. Debes comprobar este valor inmediatamente después de iniciar o detener la grabación de vídeo para asegurarte de que se admite el modo deseado en el estado de grabación de vídeo actual.


> [!NOTE] 
> A partir de Windows 10, versión 1709, cuando **AdvancedPhotoCapture** se establece en modo HDR, se ignora la configuración de la propiedad [**FlashControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.FlashControl.Enabled) y el flash nunca se activa. Para otros modos de captura, si la propiedad **FlashControl.Enabled** se establece en true, se invalidará la configuración de **AdvancedPhotoCapture** y una foto normal se capturará con flash. Si [**automática**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.FlashControl.Auto) se establece en true, **AdvancedPhotoCapture** podrá o no usar el flash, en función del comportamiento predeterminado del controlador de la cámara para las condiciones de la escena actual. En versiones anteriores, la configuración de flash **AdvancedPhotoCapture** siempre invalida la configuración de **FlashControl.Enabled**.

> [!NOTE] 
> Este artículo se basa en los conceptos y el código analizados en [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), donde se describen los pasos para implementar la captura básica de fotos y vídeo. Se recomienda que te familiarices con el patrón de captura de multimedia básico de ese artículo antes de pasar a escenarios de captura más avanzados. El código que encontrarás en este artículo se ha agregado suponiendo que la aplicación ya tiene una instancia de MediaCapture inicializada correctamente.

Existe una muestra universal de Windows que demuestra el uso de la clase **AdvancedPhotoCapture**, que puedes usar para ver la API usada en contexto o como un punto de partida para tu propia aplicación. Para obtener más información, consulta [Camera Advanced Capture sample](https://go.microsoft.com/fwlink/?LinkID=620517) (Muestra de captura de cámara avanzada).

## <a name="advanced-photo-capture-namespaces"></a>Espacios nombres de captura de fotos avanzada

Los ejemplos de código de este artículo usan las API de los espacios de nombres siguientes además de los espacios de nombres necesarios para la captura multimedia básica.

[!code-cs[HDRPhotoUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHDRPhotoUsing)]

## <a name="hdr-photo-capture"></a>Captura de fotos HDR

### <a name="determine-if-hdr-photo-capture-is-supported-on-the-current-device"></a>Determinar si se admite la captura de fotos HDR en el dispositivo actual

La técnica de captura HDR descrita en este artículo se realiza mediante el objeto [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386). No todos los dispositivos admiten la captura HDR con **AdvancedPhotoCapture**. Determina si el dispositivo en el que se está ejecutando la aplicación admite la técnica mediante la obtención del elemento [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) del objeto **MediaCapture** y luego la obtención de la propiedad [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840). Compruebe la colección [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844) del controlador del dispositivo de vídeo para ver si incluye [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845). Si es así, se admite la captura HDR con **AdvancedPhotoCapture**.

[!code-cs[HdrSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHdrSupported)]

### <a name="configure-and-prepare-the-advancedphotocapture-object"></a>Configurar y preparar el objeto AdvancedPhotoCapture

Dado que necesitarás acceder a la instancia de [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) desde varias ubicaciones dentro del código, se debe declarar una variable de miembro que contenga el objeto.

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

En la aplicación, después de inicializar el objeto **MediaCapture**, crea un objeto [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837) y establece el modo en [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845). Llama al método [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841) del objeto [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) y pasa el objeto **AdvancedPhotoCaptureSettings** que creaste.

Llama al método [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403) del objeto **MediaCapture** y pasa un objeto [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) que especifique el tipo de codificación que debe usar la captura. La clase **ImageEncodingProperties** proporciona métodos estáticos para crear las codificaciones de imagen que son compatibles con **MediaCapture**.

**PrepareAdvancedPhotoCaptureAsync** devuelve el objeto [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) que se usará para iniciar la captura de fotos. Puedes usar este objeto para registrar los controladores para los elementos [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) y [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) que se describen más adelante en este artículo.

[!code-cs[CreateAdvancedCaptureAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureAsync)]

### <a name="capture-an-hdr-photo"></a>Capturar una foto HDR

Captura una foto HDR mediante una llamada al método [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388) del objeto [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386). Este método devuelve un objeto [**AdvancedCapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/mt181378) que proporciona la foto capturada en su propiedad [**Frame**](https://msdn.microsoft.com/library/windows/apps/mt181382).

[!code-cs[CaptureHdrPhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureHdrPhotoAsync)]

La mayoría de las aplicaciones de fotografía querrán codificar la rotación de la foto capturada en el archivo de imagen para que pueda mostrarse correctamente en otros dispositivos y aplicaciones. En este ejemplo se muestra el uso de la clase auxiliar **CameraRotationHelper** para calcular la orientación correcta del archivo. Esta clase se describe y muestra en su totalidad en el artículo [**Controlar la orientación del dispositivo con MediaCapture**](handle-device-orientation-with-mediacapture.md).

El método auxiliar **SaveCapturedFrameAsync**, que guarda la imagen en el disco, se explica más adelante en este artículo.

### <a name="get-optional-reference-frame"></a>Obtener el marco de referencia opcional

El proceso HDR captura distintos marcos y, después, los compone en una sola imagen una vez que todos los marcos se han capturado. Puedes obtener acceso a un fotograma después de se capture, pero antes que todo el proceso HDR se complete mediante el control del evento [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392). No es necesario hacerlo si solamente estás interesado en el resultado final de la foto HDR.

> [!IMPORTANT]
> [**OptionalReferencePhotoCaptured** ](https://msdn.microsoft.com/library/windows/apps/mt181392) no se produce en los dispositivos que admiten hardware HDR y, por tanto, no generan fotogramas de referencia. La aplicación debe controlar el caso donde no se genera este evento.

Dado que el fotograma de referencia llega fuera del contexto de la llamada a **CaptureAsync**, se ofrece un mecanismo para pasar información del contexto al controlador **OptionalReferencePhotoCaptured**. Primero debes llamar a un objeto que contenga la información del contexto. Tú decides el nombre y el contenido de este objeto. Este ejemplo define un objeto que tiene miembros para realizar un seguimiento del nombre de archivo y de la orientación de cámara de la captura.

[!code-cs[AdvancedCaptureContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAdvancedCaptureContext)]

Crea una nueva instancia del objeto de contexto, rellena sus miembros y luego pásala a la sobrecarga de [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388) que acepta un objeto como un parámetro.

[!code-cs[CaptureWithContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureWithContext)]

En el controlador de eventos [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392), convierte la propiedad [**Context**](https://msdn.microsoft.com/library/windows/apps/mt181405) del objeto [**OptionalReferencePhotoCapturedEventArgs**](https://msdn.microsoft.com/library/windows/apps/mt181404) a la clase del objeto de contexto. En este ejemplo se modifica el nombre de archivo para distinguir la imagen del fotograma de referencia de la imagen HDR final y después se llama al método auxiliar **SaveCapturedFrameAsync** para guardar la imagen.

[!code-cs[OptionalReferencePhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOptionalReferencePhotoCaptured)]

### <a name="receive-a-notification-when-all-frames-have-been-captured"></a>Recibir una notificación cuando se han capturado todos los fotogramas

La captura de fotos HDR tiene dos pasos. En primer lugar, se capturan varios marcos y, a continuación, se procesan los marcos en la imagen HDR final. No puedes iniciar otra captura mientras se sigan capturando los fotogramas HDR de origen, pero puedes iniciar una captura después de que se han capturado todos los fotogramas, si bien antes de que finalice el posprocesamiento HDR. El evento [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) se genera cuando las capturas HDR se completan, lo que te permite saber que puedes iniciar otra captura. Un escenario típico consiste en deshabilitar el botón de captura de la interfaz de usuario cuando comienza la captura HDR para después volver a habilitarlo cuando se genera **AllPhotosCaptured**.

[!code-cs[AllPhotosCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAllPhotosCaptured)]

### <a name="clean-up-the-advancedphotocapture-object"></a>Limpiar el objeto AdvancedPhotoCapture

Cuando la aplicación haya terminado de capturar, antes de eliminar el objeto **MediaCapture**, debes apagar el objeto [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) con una llamada a [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/mt181391) y establecer la variable de miembro en null.

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]


## <a name="low-light-photo-capture"></a>Captura de fotos con poca luz
A partir de Windows 10, versión 1607, se puede usar la clase **AdvancedPhotoCapture** para capturar fotos con un algoritmo integrado que mejora la calidad de las fotos que se capturan con configuración de poca luz. Cuando uses la característica de poca luz de la clase [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture), el sistema evaluará la escena actual y, si es necesario, aplicará un algoritmo para compensar las condiciones de poca luz. Si el sistema determina que el algoritmo no es necesario, se realiza una captura normal en su lugar.

Antes de usar una captura de fotos con poca luz, determina si el dispositivo en el que se está ejecutando la aplicación admite la técnica mediante la obtención del elemento [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) del objeto **MediaCapture** y, luego, la obtención de la propiedad [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840). Comprueba la colección [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844) del controlador del dispositivo de vídeo para ver si incluye [**AdvancedPhotoMode.LowLight**](https://msdn.microsoft.com/library/windows/apps/mt147845). Si es así, se admite la captura con poca luz mediante **AdvancedPhotoCapture**. 
[!code-cs[LowLightSupported1](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetLowLightSupported1)]

[!code-cs[LowLightSupported2](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetLowLightSupported2)]

A continuación, declara una variable de miembro para almacenar el objeto **AdvancedPhotoCapture**. 

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

En la aplicación, después de inicializar el objeto **MediaCapture**, crea un objeto [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837) y establece el modo en [**AdvancedPhotoMode.LowLight**](https://msdn.microsoft.com/library/windows/apps/mt147845). Llama al método [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841) del objeto [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) y pasa el objeto **AdvancedPhotoCaptureSettings** que creaste.

Llama al método [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403) del objeto **MediaCapture** y pasa un objeto [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) que especifique el tipo de codificación que debe usar la captura. 

[!code-cs[CreateAdvancedCaptureLowLightAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureLowLightAsync)]

Para capturar una foto, llama a [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.CaptureAsync).

[!code-cs[CaptureLowLight](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureLowLight)]

Como en el ejemplo de HDR anterior, en este ejemplo se usa una clase auxiliar denominada **CameraRotationHelper** para determinar el valor de rotación que se debe codificar en la imagen para que pueda mostrarse correctamente en otros dispositivos y aplicaciones. Esta clase se describe y muestra en su totalidad en el artículo [**Controlar la orientación del dispositivo con MediaCapture**](handle-device-orientation-with-mediacapture.md).

El método auxiliar **SaveCapturedFrameAsync**, que guarda la imagen en el disco, se explica más adelante en este artículo.

Puedes capturar varias fotos con poca luz sin volver a configurar el objeto **AdvancedPhotoCapture**, pero, al terminar la captura, debes llamar a [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.FinishAsync) para limpiar el objeto y los recursos asociados.

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]

## <a name="working-with-advancedcapturedphoto-objects"></a>Trabajo con objetos AdvancedCapturedPhoto
[**AdvancedPhotoCapture.CaptureAsync** ](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.CaptureAsync) devuelve un [ **AdvancedCapturedPhoto** ](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedCapturedPhoto) objeto que representa la imagen capturada. Este objeto expone la propiedad [**Frame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedCapturedPhoto.Frame), que devuelve un objeto [**CapturedFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame) que representa la imagen. El evento [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.OptionalReferencePhotoCaptured) también proporciona un objeto **CapturedFrame** en sus argumentos. Después de obtener un objeto de este tipo, hay una serie de cosas que puedes hacer con él, como crear un objeto [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap) o guardar la imagen en un archivo. 

## <a name="get-a-softwarebitmap-from-a-capturedframe"></a>Obtener un objeto SoftwareBitmap de un objeto CapturedFrame
Para obtener un objeto **SoftwareBitmap** de un objeto **CapturedFrame** fácilmente, solo tienes que acceder a la propiedad [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame.SoftwareBitmap) del objeto. Sin embargo, la mayoría de los formatos de codificación no admiten **SoftwareBitmap** con **AdvancedPhotoCapture**, por lo que debes comprobar que la propiedad no sea nula antes de usarla.

[!code-cs[SoftwareBitmapFromCapturedFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmapFromCapturedFrame)]

En la versión actual, el único formato de codificación que admite **SoftwareBitmap** para **AdvancedPhotoCapture** es NV12 sin comprimir. Por lo tanto, si quieres usar esta característica, debes especificar esa codificación cuando llames a [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403). 

[!code-cs[UncompressedNv12](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUncompressedNv12)]

Por supuesto, siempre puedes guardar la imagen en un archivo y, luego, cargar el archivo en un objeto **SoftwareBitmap** en otro paso. Para obtener más información sobre el uso del objeto **SoftwareBitmap**, consulta [**Crear, editar y guardar imágenes de mapa de bits**](imaging.md).

## <a name="save-a-capturedframe-to-a-file"></a>Guardar un objeto CapturedFrame en un archivo
La clase [**CapturedFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame) implementa la interfaz IInputStream, de modo que puede usarse como entrada para una clase [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder); posteriormente, una clase [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder) puede usarse para escribir los datos de imagen en el disco.

En el ejemplo siguiente, se crea una carpeta nueva en la biblioteca de imágenes del usuario y se crea un archivo dentro de esta carpeta. Ten en cuenta que la aplicación deberá incluir la funcionalidad **Biblioteca de imágenes** en el archivo de manifiesto de la aplicación para poder acceder a este directorio. A continuación, se abre una secuencia de archivos en el archivo especificado. Después, se llama a [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder.CreateAsync) para crear el descodificador desde la clase **CapturedFrame**. Luego, [**CreateForTranscodingAsync**](https://msdn.microsoft.com/library/windows/apps/br226214) crea un codificador desde la secuencia de archivos y el descodificador.

En los pasos siguientes se codifica la orientación de la foto en el archivo de imagen mediante el método [**BitmapProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.BitmapProperties) del codificador. Para obtener más información sobre cómo controlar la orientación al capturar imágenes, consulta [**Controlar la orientación del dispositivo con MediaCapture**](handle-device-orientation-with-mediacapture.md).

Por último, la imagen se escribe en el archivo con una llamada a [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.FlushAsync).

[!code-cs[SaveCapturedFrameAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSaveCapturedFrameAsync)]

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Capturar básica de fotos, vídeo y audio con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
