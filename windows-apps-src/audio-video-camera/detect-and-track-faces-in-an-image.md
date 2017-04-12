---
author: drewbatgit
ms.assetid: 84729E44-10E9-4D7D-8575-6A9D97467ECD
description: "Este tema muestra cómo usar el FaceDetector para detectar los rostros de una imagen El FaceTracker está optimizado para realizar el seguimiento facial durante una secuencia de fotogramas de vídeo."
title: "Detectar rostros en imágenes o vídeos"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 3af1a18f7ceceb359c0545293f2b82ec9fd53c09
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="detect-faces-in-images-or-videos"></a>Detectar rostros en imágenes o vídeos

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


\[Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.\]

Este tema muestra cómo usar el [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) para detectar los rostros de una imagen El [**FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150) está optimizado para realizar el seguimiento facial durante una secuencia de fotogramas de vídeo.

Para conocer un método alternativo de seguimiento facial mediante el [**FaceDetectionEffect**](https://msdn.microsoft.com/library/windows/apps/dn948776), consulta [Análisis de la escena para la captura multimedia](scene-analysis-for-media-capture.md).

El código de este artículo se adaptó de los ejemplos de [detección de rostro](http://go.microsoft.com/fwlink/p/?LinkId=620512&clcid=0x409) y [seguimiento facial básico](http://go.microsoft.com/fwlink/p/?LinkId=620513&clcid=0x409). Puedes descargar estos ejemplos para ver el código usado en contexto o para usarlos como punto de partida para tu propia aplicación.

## <a name="detect-faces-in-a-single-image"></a>Detectar rostros en una sola imagen

La clase [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) permite detectar uno o más rostros en una imagen estática.

Este ejemplo usa las API de los siguientes espacios de nombres.

[!code-cs[FaceDetectionUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceDetectionUsing)]

Declara una variable de miembro de clase para el objeto [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) y para la lista de objetos [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123) que se detectarán en la imagen.

[!code-cs[ClassVariables1](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables1)]

La detección de rostro funciona en un objeto [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) que se puede crear de distintas maneras. En este ejemplo, se usa un [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) para permitir al usuario seleccionar un archivo de imagen en el que se detectarán rostros. Para obtener información sobre cómo trabajar con mapas de bits de software, consulta [Imágenes](imaging.md).

[!code-cs[Selector](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetPicker)]

Usa la clase [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) para descodificar el archivo de imagen en un **SoftwareBitmap**. El proceso de detección de rostro es más rápido con una imagen más pequeña, por lo que quizás desees escalar la imagen de origen a un tamaño menor. Esto puede realizarse durante la descodificación mediante la creación de un objeto [**BitmapTransform**](https://msdn.microsoft.com/library/windows/apps/br226254), la definición de las propiedades [**ScaledWidth**](https://msdn.microsoft.com/library/windows/apps/br226261) y [**ScaledHeight**](https://msdn.microsoft.com/library/windows/apps/br226260), y su paso a la llamada a [**GetSoftwareBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn887332), que devuelve el **SoftwareBitmap** descodificado y escalado.

[!code-cs[Descodificar](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDecode)]

En la versión actual, la clase **FaceDetector** solo admite imágenes en Gray8 o Nv12. La clase **SoftwareBitmap** proporciona el método [**Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362), que convierte un mapa de bits de un formato a otro. En este ejemplo se convierte la imagen de origen en el formato de píxel Gray8 si aún no está en ese formato. Si quieres, puedes usar los métodos [**GetSupportedBitmapPixelFormats**](https://msdn.microsoft.com/library/windows/apps/dn974140) y [**IsBitmapPixelFormatSupported**](https://msdn.microsoft.com/library/windows/apps/dn974142) para determinar en tiempo de ejecución si se admite un formato de píxel, en caso de que el conjunto de formatos compatibles se expanda en futuras versiones.

[!code-cs[Formato](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFormat)]

Crea una instancia del objeto **FaceDetector** mediante una llamada a [**CreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn974132) y luego llama a [**DetectFacesAsync**](https://msdn.microsoft.com/library/windows/apps/dn974134) y pasa el mapa de bits que se ha escalado a un tamaño razonable y se ha convertido a un formato de píxel admitido. Este método devuelve una lista de objetos [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123). A continuación se muestra el método auxiliar **ShowDetectedFaces**, que dibuja cuadrados alrededor de los rostros de la imagen.

[!code-cs[Detectar](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDetect)]

Asegúrate de eliminar los objetos que se crearon durante el proceso de detección de rostro.

[!code-cs[Dispose](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDispose)]

Para mostrar la imagen y dibujar cuadros alrededor de los rostros detectados, agrega un elemento [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) a la página XAML.

[!code-xml[Lienzo](./code/FaceDetection_Win10/cs/MainPage.xaml#SnippetCanvas)]

Define algunas variables de miembro para aplicar estilo a los cuadrados que se dibujarán.

[!code-cs[ClassVariables2](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables2)]

En el método auxiliar **ShowDetectedFaces**, se crea un nuevo [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101) y se establece el origen en un [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) creado a partir del **SoftwareBitmap** que representa la imagen de origen. El fondo del control **Canvas** XAML se establece en el pincel de imagen.

Si la lista de rostros pasada al método auxiliar no está vacía, recorre cada rostro de la lista y usa la propiedad [**FaceBox**](https://msdn.microsoft.com/library/windows/apps/dn974126) de la clase [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123) para determinar la posición y el tamaño del rectángulo dentro de la imagen que contiene el rostro. Dada la alta probabilidad de que el control **Canvas** tenga un tamaño diferente al de la imagen de origen, debes multiplicar las coordenadas X e Y, así como el ancho y el alto, de la propiedad **FaceBox** por un valor de escalado que sea la proporción entre el tamaño de la imagen de origen y el tamaño real del control **Canvas**.

[!code-cs[ShowDetectedFaces](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetShowDetectedFaces)]

## <a name="track-faces-in-a-sequence-of-frames"></a>Realizar el seguimiento facial en una secuencia de fotogramas

Si quieres detectar rostros en un vídeo, es más eficaz usar la clase [**FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150) que la clase [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129), aunque los pasos de implementación son muy similares. El **FaceTracker** usa la información sobre los fotogramas procesados anteriormente para optimizar el proceso de detección.

[!code-cs[FaceTrackingUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceTrackingUsing)]

Declara una variable de clase para el objeto **FaceTracker**. En este ejemplo se usa un [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/br230587) para iniciar el seguimiento facial en un intervalo definido. Una clase [SemaphoreSlim](https://msdn.microsoft.com/library/system.threading.semaphoreslim.aspx) se usa para garantizar que solo se ejecute una operación de seguimiento facial a la vez.

[!code-cs[ClassVariables3](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables3)]

Para inicializar la operación de seguimiento facial, crea un nuevo objeto **FaceTracker** mediante una llamada a [**CreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn974151). Inicializa el intervalo del temporizador deseado y, luego, crea el temporizador. Se llamará al método auxiliar **ProcessCurrentVideoFrame** cada vez que transcurra el intervalo especificado.

[!code-cs[TrackingInit](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetTrackingInit)]

El temporizador llama de forma asincrónica al método auxiliar **ProcessCurrentVideoFrame**, por lo que el método llama primero al método **Wait** del semáforo para comprobar si existe una operación de seguimiento facial en curso y, en ese caso, el método se devuelve sin intentar detectar rostros. Al final de este método, se llama al método **Release** del semáforo, lo que permite continuar con la llamada posterior a **ProcessCurrentVideoFrame**.

La clase [**FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150) funciona en objetos [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917). Existen varias formas de obtener un **VideoFrame**, incluidas la captura de un fotograma de vista previa de un objeto [MediaCapture](capture-photos-and-video-with-mediacapture.md) en ejecución o la implementación del método [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764784) del [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788). En este ejemplo se usa un método auxiliar no definido que devuelve un fotograma de vídeo, **GetLatestFrame**, como un marcador de posición para esta operación. Para obtener información sobre cómo obtener fotogramas de vídeo de la secuencia de vista previa de un dispositivo de captura multimedia en ejecución, consulta [Obtener un marco de vista previa](get-a-preview-frame.md).

Al igual que con **FaceDetector**, el **FaceTracker** admite un conjunto limitado de formatos de píxel. En este ejemplo se abandona la detección de rostro si el fotograma suministrado no está en el formato Nv12.

Llama a [**ProcessNextFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn974157) para recuperar una lista de objetos [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123) que representen los rostros del fotograma. Cuando tengas la lista de rostros, puedes mostrarlos del mismo modo descrito anteriormente para la detección de rostros. Ten en cuenta que, dado que el método auxiliar de seguimiento facial no se llama en el subproceso de la interfaz de usuario, debes realizar las actualizaciones de la interfaz de usuario en un [**CoredDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) de llamada.

[!code-cs[ProcessCurrentVideoFrame](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetProcessCurrentVideoFrame)]

## <a name="related-topics"></a>Temas relacionados

* [Análisis de la escena para la captura multimedia](scene-analysis-for-media-capture.md)
* [Ejemplo de detección de rostro básica](http://go.microsoft.com/fwlink/p/?LinkId=620512&clcid=0x409)
* [Ejemplo de seguimiento facial básico](http://go.microsoft.com/fwlink/p/?LinkId=620513&clcid=0x409)
* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Reproducción de contenido multimedia](media-playback.md)
