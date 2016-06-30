---
author: drewbatgit
ms.assetid: 0186EA01-8446-45BA-A109-C5EB4B80F368
description: "La clase AdvancedPhotoCapture permite capturar fotos de alto intervalo dinámico (HDR)."
title: "Captura de fotos de alto intervalo dinámico (HDR)"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 3015aa4338ddb0c0a006eb631026261a4453f376

---

# Captura de fotos de alto intervalo dinámico (HDR)

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


La clase [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) permite capturar fotos de alto intervalo dinámico (HDR). Esta API también te permite obtener un marco de referencia de la captura HDR antes de que finalice el procesamiento de la imagen final.

Otros artículos relacionados con la captura HDR incluyen:

-   Puedes usar la clase [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/dn948902) para permitir que el sistema evalúe el contenido del flujo de vista previa de la captura multimedia para determinar si el procesamiento HDR mejoraría el resultado de la captura. Para más información, consulta [Análisis de la escena para la captura multimedia](scene-analysis-for-media-capture.md).

-   Usa la clase [**HdrVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926680) para capturar vídeo con el algoritmo de procesamiento HDR integrado de Windows. Para más información, consulta [Capturar controles del dispositivo de captura de vídeo](capture-device-controls-for-video-capture.md).

-   Puedes usar la clase [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564) para capturar una secuencia de fotos, cada una con configuraciones de captura diferentes, e implementar un tipo de HDR propio u otro algoritmo de procesamiento. Para más información, consulta [Secuencia de fotos variable](variable-photo-sequence.md).

**Nota**
-   No se admite la grabación de vídeo y la captura de fotos simultánea con **AdvancedPhotoCapture**.

-   No se puede usar el flash de la cámara durante la captura de fotos avanzada.

**Nota** Este artículo se basa en los conceptos y el código planteados en [Capturar fotos y vídeo con MediaCapture](capture-photos-and-video-with-mediacapture.md), donde se describen los pasos para la implementación básica de fotos y la captura de vídeo. Se recomienda que te familiarices con el patrón de captura de multimedia básico de ese artículo antes de pasar a escenarios más avanzados de captura. El código de este artículo supone que la aplicación ya tiene una instancia de MediaCapture inicializada correctamente.

## Espacios de nombres de captura de fotos HDR

Para usar la captura de fotos HDR, la aplicación debe incluir los siguientes espacios de nombres además de los espacios de nombres necesarios para la captura multimedia básica.

[!code-cs[HDRPhotoUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHDRPhotoUsing)]


## Determinar si se admite la captura de fotos HDR en el dispositivo actual

La técnica de captura HDR descrita en este artículo se realiza mediante el objeto [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386). No todos los dispositivos admiten la captura HDR con **AdvancedPhotoCapture**. Determina si el dispositivo en el que se está ejecutando la aplicación admite la técnica mediante la obtención del elemento [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) del objeto **MediaCapture** y luego la obtención de la propiedad [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840). Compruebe la colección [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844) del controlador del dispositivo de vídeo para ver si incluye [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845). Si es así, se admite la captura HDR con **AdvancedPhotoCapture**.

[!code-cs[HdrSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHdrSupported)]

## Configurar y preparar el objeto AdvancedPhotoCapture

Como es necesario acceder a la instancia de [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) desde varias ubicaciones dentro del código, se debe declarar una variable de miembro que contenga el objeto.

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

En la aplicación, después de inicializar el objeto **MediaCapture**, crea un objeto [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837) y establece el modo en [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845). Llama al método [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841) del objeto [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) y pasa el objeto **AdvancedPhotoCaptureSettings** que creaste.

Llama al método [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403) del objeto **MediaCapture** y pasa un objeto [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) que especifique el tipo de codificación que debe usar la captura. La clase **ImageEncodingProperties** proporciona métodos estáticos para crear las codificaciones de imagen que son compatibles con **MediaCapture**.

**PrepareAdvancedPhotoCaptureAsync** devuelve el objeto [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) que se usará para iniciar la captura de fotos. Puedes usar este objeto para registrar los controladores para los elementos [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) y [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) que se describen más adelante en este artículo.

[!code-cs[CreateAdvancedCaptureAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureAsync)]

## Capturar una foto HDR

Captura una foto HDR mediante una llamada al método [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388) del objeto [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386). Este método devuelve un objeto [**AdvancedCapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/mt181378) que proporciona la foto capturada en su propiedad [**Frame**](https://msdn.microsoft.com/library/windows/apps/mt181382).

[!code-cs[CaptureHdrPhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureHdrPhotoAsync)]

**ConvertOrientationToPhotoOrientation** y **ReencodeAndSavePhotoAsync** son métodos auxiliares que se tratan como parte del escenario de captura de multimedia básico del artículo [Capturar fotos y vídeo con MediaCapture](capture-photos-and-video-with-mediacapture.md).

## Obtener el marco de referencia opcional

El proceso HDR captura distintos marcos y después los compone en una sola imagen una vez que todos de los marcos se han capturado. Puedes obtener acceso a un fotograma después de se capture, pero antes que todo el proceso HDR se complete mediante el control del evento [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392). No es necesario hacerlo si solamente estás interesado en el resultado final de la foto HDR.

**Importante**
            [
              **OptionalReferencePhotoCaptured**
            ](https://msdn.microsoft.com/library/windows/apps/mt181392) no se genera en dispositivos que admitan hardware HDR y, por tanto, no se generan los fotogramas de referencia.. La aplicación debe controlar el caso donde no se genera este evento.

Como el fotograma de referencia llega fuera del contexto de la llamada a **CaptureAsync**, se ofrece un mecanismo para pasar información del contexto al controlador **OptionalReferencePhotoCaptured**. Primero debes un objeto que contenga la información de contexto. El nombre y el contenido de este objeto depende de TI. Este ejemplo define un objeto que tiene miembros para realizar un seguimiento del nombre de archivo y de la orientación de cámara de la captura.

[!code-cs[AdvancedCaptureContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAdvancedCaptureContext)]

Crea una nueva instancia del objeto de contexto, rellena sus miembros y luego pásala a la sobrecarga de [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388) que acepta un objeto como un parámetro.

[!code-cs[CaptureWithContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureWithContext)]

En el controlador de eventos [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392), convierte la propiedad [**Context**](https://msdn.microsoft.com/library/windows/apps/mt181405) del objeto [**OptionalReferencePhotoCapturedEventArgs**](https://msdn.microsoft.com/library/windows/apps/mt181404) a la clase del objeto de contexto. En este ejemplo se modifica el nombre de archivo para distinguir la imagen del fotograma de referencia de la imagen HDR final y después se llama al método auxiliar **ReencodeAndSavePhotoAsync** para guardar la imagen.

[!code-cs[OptionalReferencePhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOptionalReferencePhotoCaptured)]

## Recibir una notificación cuando se han capturado todos los fotogramas

La captura de fotos HDR tiene dos pasos. En primer lugar, se capturan varios marcos y, a continuación, se procesan los marcos en la imagen HDR final. No puedes iniciar otra captura mientras se sigan capturando los fotogramas HDR de origen, pero puedes iniciar una captura después de que se han capturado todos los fotogramas, si bien antes de que finalice el posprocesamiento HDR. El evento [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) se genera cuando las capturas HDR se completan, lo que te permite saber que puedes iniciar otra captura. Un escenario típico consiste en deshabilitar el botón de captura de la interfaz de usuario cuando comienza la captura HDR para después volver a habilitarlo cuando se genera **AllPhotosCaptured**.

[!code-cs[AllPhotosCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAllPhotosCaptured)]

## Limpiar el objeto AdvancedPhotoCapture

Cuando la aplicación haya terminado de capturar, antes de eliminar el objeto **MediaCapture**, debes apagar el objeto [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) con una llamada a [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/mt181391) y establecer la variable de miembro en null.

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]

## Temas relacionados

* [Capturar fotografías y vídeos con MediaCapture](capture-photos-and-video-with-mediacapture.md)



<!--HONumber=Jun16_HO4-->


