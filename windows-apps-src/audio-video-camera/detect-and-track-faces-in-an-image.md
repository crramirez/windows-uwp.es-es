---
ms.assetid: 84729E44-10E9-4D7D-8575-6A9D97467ECD
description: Este tema muestra cómo usar el FaceDetector para detectar los rostros de una imagen El FaceTracker está optimizado para realizar el seguimiento facial durante una secuencia de fotogramas de vídeo.
title: Detectar rostros en imágenes o vídeos
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b6b2cf937646f41a2e51e6a109d272965817665f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160889"
---
# <a name="detect-faces-in-images-or-videos"></a>Detectar rostros en imágenes o vídeos



\[Algunos datos se relacionan con productos de versiones preliminares que pueden modificarse sustancialmente antes de su lanzamiento comercial. Microsoft no otorga ninguna garantía, explícita o implícita, con respecto a la información proporcionada aquí.\]

Este tema muestra cómo usar el [**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) para detectar los rostros de una imagen El [**FaceTracker**](/uwp/api/Windows.Media.FaceAnalysis.FaceTracker) está optimizado para realizar el seguimiento facial durante una secuencia de fotogramas de vídeo.

Para conocer un método alternativo de seguimiento facial mediante el [**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect), consulta [Análisis de la escena para la captura multimedia](scene-analysis-for-media-capture.md).

El código de este artículo se adaptó de los ejemplos de [detección de rostro](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicFaceDetection) y [seguimiento facial básico](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicFaceTracking). Puedes descargar estos ejemplos para ver el código usado en contexto o para usarlos como punto de partida para tu propia aplicación.

## <a name="detect-faces-in-a-single-image"></a>Detectar rostros en una sola imagen

La clase [**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) permite detectar uno o más rostros en una imagen estática.

Este ejemplo usa las API de los siguientes espacios de nombres.

[!code-cs[FaceDetectionUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceDetectionUsing)]

Declara una variable de miembro de clase para el objeto [**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) y para la lista de objetos [**DetectedFace**](/uwp/api/Windows.Media.FaceAnalysis.DetectedFace) que se detectarán en la imagen.

[!code-cs[ClassVariables1](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables1)]

La detección de rostro funciona en un objeto [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) que se puede crear de distintas maneras. En este ejemplo, se usa un [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) para permitir al usuario seleccionar un archivo de imagen en el que se detectarán rostros. Para obtener información sobre cómo trabajar con mapas de bits de software, consulta [Imágenes](imaging.md).

[!code-cs[Picker](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetPicker)]

Usa la clase [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) para descodificar el archivo de imagen en un **SoftwareBitmap**. El proceso de detección de rostro es más rápido con una imagen más pequeña, por lo que quizás desees escalar la imagen de origen a un tamaño menor. Esto puede realizarse durante la descodificación mediante la creación de un objeto [**BitmapTransform**](/uwp/api/Windows.Graphics.Imaging.BitmapTransform), la definición de las propiedades [**ScaledWidth**](/uwp/api/windows.graphics.imaging.bitmaptransform.scaledwidth) y [**ScaledHeight**](/uwp/api/windows.graphics.imaging.bitmaptransform.scaledheight), y su paso a la llamada a [**GetSoftwareBitmapAsync**](/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync), que devuelve el **SoftwareBitmap** descodificado y escalado.

[!code-cs[Decode](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDecode)]

En la versión actual, la clase **FaceDetector** solo admite imágenes en Gray8 o Nv12. La clase **SoftwareBitmap** proporciona el método [**Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert), que convierte un mapa de bits de un formato a otro. En este ejemplo se convierte la imagen de origen en el formato de píxel Gray8 si aún no está en ese formato. Si quieres, puedes usar los métodos [**GetSupportedBitmapPixelFormats**](/uwp/api/windows.media.faceanalysis.facedetector.getsupportedbitmappixelformats) y [**IsBitmapPixelFormatSupported**](/uwp/api/windows.media.faceanalysis.facedetector.isbitmappixelformatsupported) para determinar en tiempo de ejecución si se admite un formato de píxel, en caso de que el conjunto de formatos compatibles se expanda en futuras versiones.

[!code-cs[Format](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFormat)]

Crea una instancia del objeto **FaceDetector** mediante una llamada a [**CreateAsync**](/uwp/api/windows.media.faceanalysis.facedetector.createasync) y luego llama a [**DetectFacesAsync**](/uwp/api/windows.media.faceanalysis.facedetector.detectfacesasync) y pasa el mapa de bits que se ha escalado a un tamaño razonable y se ha convertido a un formato de píxel admitido. Este método devuelve una lista de objetos [**DetectedFace**](/uwp/api/Windows.Media.FaceAnalysis.DetectedFace). A continuación se muestra el método auxiliar **ShowDetectedFaces**, que dibuja cuadrados alrededor de los rostros de la imagen.

[!code-cs[Detect](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDetect)]

Asegúrate de eliminar los objetos que se crearon durante el proceso de detección de rostro.

[!code-cs[Dispose](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDispose)]

Para mostrar la imagen y dibujar cuadros alrededor de los rostros detectados, agrega un elemento [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) a la página XAML.

[!code-xml[Canvas](./code/FaceDetection_Win10/cs/MainPage.xaml#SnippetCanvas)]

Define algunas variables de miembro para aplicar estilo a los cuadrados que se dibujarán.

[!code-cs[ClassVariables2](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables2)]

En el método auxiliar **ShowDetectedFaces**, se crea un nuevo [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) y se establece el origen en un [**SoftwareBitmapSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) creado a partir del **SoftwareBitmap** que representa la imagen de origen. El fondo del control **Canvas** XAML se establece en el pincel de imagen.

Si la lista de rostros pasada al método auxiliar no está vacía, recorre cada rostro de la lista y usa la propiedad [**FaceBox**](/uwp/api/windows.media.faceanalysis.detectedface.facebox) de la clase [**DetectedFace**](/uwp/api/Windows.Media.FaceAnalysis.DetectedFace) para determinar la posición y el tamaño del rectángulo dentro de la imagen que contiene el rostro. Dada la alta probabilidad de que el control **Canvas** tenga un tamaño diferente al de la imagen de origen, debes multiplicar las coordenadas X e Y, así como el ancho y el alto, de la propiedad **FaceBox** por un valor de escalado que sea la proporción entre el tamaño de la imagen de origen y el tamaño real del control **Canvas**.

[!code-cs[ShowDetectedFaces](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetShowDetectedFaces)]

## <a name="track-faces-in-a-sequence-of-frames"></a>Realizar el seguimiento facial en una secuencia de fotogramas

Si quieres detectar rostros en un vídeo, es más eficaz usar la clase [**FaceTracker**](/uwp/api/Windows.Media.FaceAnalysis.FaceTracker) que la clase [**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector), aunque los pasos de implementación son muy similares. El **FaceTracker** usa la información sobre los fotogramas procesados anteriormente para optimizar el proceso de detección.

[!code-cs[FaceTrackingUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceTrackingUsing)]

Declara una variable de clase para el objeto **FaceTracker**. En este ejemplo se usa un [**ThreadPoolTimer**](/uwp/api/Windows.System.Threading.ThreadPoolTimer) para iniciar el seguimiento facial en un intervalo definido. Una clase [SemaphoreSlim](/dotnet/api/system.threading.semaphoreslim) se usa para garantizar que solo se ejecute una operación de seguimiento facial a la vez.

[!code-cs[ClassVariables3](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables3)]

Para inicializar la operación de seguimiento facial, crea un nuevo objeto **FaceTracker** mediante una llamada a [**CreateAsync**](/uwp/api/windows.media.faceanalysis.facetracker.createasync). Inicializa el intervalo del temporizador deseado y, luego, crea el temporizador. Se llamará al método auxiliar **ProcessCurrentVideoFrame** cada vez que transcurra el intervalo especificado.

[!code-cs[TrackingInit](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetTrackingInit)]

El temporizador llama de forma asincrónica al método auxiliar **ProcessCurrentVideoFrame**, por lo que el método llama primero al método **Wait** del semáforo para comprobar si existe una operación de seguimiento facial en curso y, en ese caso, el método se devuelve sin intentar detectar rostros. Al final de este método, se llama al método **Release** del semáforo, lo que permite continuar con la llamada posterior a **ProcessCurrentVideoFrame**.

La clase [**FaceTracker**](/uwp/api/Windows.Media.FaceAnalysis.FaceTracker) funciona en objetos [**VideoFrame**](/uwp/api/Windows.Media.VideoFrame). Existen varias formas de obtener un **VideoFrame**, incluidas la captura de un fotograma de vista previa de un objeto [MediaCapture](./index.md) en ejecución o la implementación del método [**ProcessFrame**](/uwp/api/windows.media.effects.ibasicaudioeffect.processframe) del [**IBasicVideoEffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect). En este ejemplo se usa un método auxiliar no definido que devuelve un fotograma de vídeo, **GetLatestFrame**, como un marcador de posición para esta operación. Para obtener información sobre cómo obtener fotogramas de vídeo de la secuencia de vista previa de un dispositivo de captura multimedia en ejecución, consulta [Obtener un marco de vista previa](get-a-preview-frame.md).

Al igual que con **FaceDetector**, el **FaceTracker** admite un conjunto limitado de formatos de píxel. En este ejemplo se abandona la detección de rostro si el fotograma suministrado no está en el formato Nv12.

Llama a [**ProcessNextFrameAsync**](/uwp/api/windows.media.faceanalysis.facetracker.processnextframeasync) para recuperar una lista de objetos [**DetectedFace**](/uwp/api/Windows.Media.FaceAnalysis.DetectedFace) que representen los rostros del fotograma. Cuando tengas la lista de rostros, puedes mostrarlos del mismo modo descrito anteriormente para la detección de rostros. Tenga en cuenta que, dado que no se llama al método auxiliar de seguimiento facial en el subproceso de interfaz de usuario, debe realizar todas las actualizaciones de la interfaz de usuario en dentro de una llamada a [**CoreDispatcher. RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync).

[!code-cs[ProcessCurrentVideoFrame](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetProcessCurrentVideoFrame)]

## <a name="related-topics"></a>Temas relacionados

* [Análisis de la escena para la captura multimedia](scene-analysis-for-media-capture.md)
* [Ejemplo de detección de rostro básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicFaceDetection)
* [Ejemplo de seguimiento facial básico](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicFaceTracking)
* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Reproducción de multimedia](media-playback.md)