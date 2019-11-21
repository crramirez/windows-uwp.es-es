---
ms.assetid: ''
description: En este artículo se muestra cómo usar la Open Source Computer Vision Library (OpenCV) con la clase MediaFrameReader.
title: Usar OpenCV con MediaFrameReader
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, openCV
ms.localizationpriority: medium
ms.openlocfilehash: a6594898dff1bf5f2262034b10e262082335f1b2
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74255953"
---
# <a name="use-the-open-source-computer-vision-library-opencv-with-mediaframereader"></a>Usar la Open Source Computer Vision Library (OpenCV) con MediaFrameReader

En este artículo se muestra cómo usar la Open Source Computer Vision Library (OpenCV), una biblioteca de código nativo que proporciona una amplia variedad de algoritmos de procesamiento de imágenes con la clase [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) que puede leer fotogramas multimedia de varias fuentes de forma simultánea. El código de ejemplo de este artículo te guía a través de la creación de una aplicación sencilla que obtiene los fotogramas de un sensor de color, desenfoca cada fotograma mediante la biblioteca de OpenCV y, luego, muestra la imagen procesada en un control **Image** XAML. 

>[!NOTE]
>OpenCV.Win.Core and OpenCV.Win.ImgProc are not regularly updated and do not pass the Store compliance checks, therefore these packages are intended for experimentation only.

Este artículo se basa en el contenido de los otros dos artículos:

* [Procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md): en este artículo se ofrece información detallada sobre cómo usar **MediaFrameReader** para obtener fotogramas de uno o varios orígenes de fotogramas multimedia y describe, en detalles, la mayoría del código de muestra de este artículo. En concreto, **Procesar fotogramas multimedia con MediaFrameReader** ofrece la lista de códigos para una clase auxiliar, **FrameRenderer**, que controla la presentación de fotogramas multimedia en un elemento **Image** XAML. El código de muestra de este artículo usa también esta clase auxiliar.

* [Process software bitmaps with OpenCV](process-software-bitmaps-with-opencv.md) - This article walks you through creating a native code Windows Runtime component, **OpenCVBridge**, which helps to convert between the **SoftwareBitmap** object, used by the **MediaFrameReader**,  and the **Mat** type used by the OpenCV library. El código de muestra de este artículo da por hecho que has seguido los pasos para agregar el componente **OpenCVBridge** a la solución de aplicación para UWP.

Además de estos artículos, para ver y descargar una muestra de código completa e integral del escenario descrito en este artículo, consulta el [Fotogramas de Cámara+ Muestra de OpenCV](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV) en el repositorio de GitHub de muestras universales de Windows.

To get started developing quickly, you can include the OpenCV library in a UWP app project by using NuGet packages, but these packages may not pass the app certficication process when you submit your app to the Store, so it is recommended that you download the OpenCV library source code and build the binaries yourself before submitting your app. Podrás encontrar información sobre el desarrollo con OpenCV en [https://opencv.org](https://opencv.org).


## <a name="implement-the-opencvhelper-native-windows-runtime-component"></a>Implement the OpenCVHelper native Windows Runtime component
Follow the steps in [Process software bitmaps with OpenCV](process-software-bitmaps-with-opencv.md) to create the OpenCV helper Windows Runtime component and add a reference to the component project to your UWP app solution.

## <a name="find-available-frame-source-groups"></a>Buscar grupos de origen de fotogramas disponibles
En primer lugar, debes encontrar un grupo de origen de fotogramas multimedia del que se obtendrán dichos fotogramas. Obtenga la lista de grupos de origen disponibles en el dispositivo actual llamando a **[MediaFrameSourceGroup.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)** . A continuación, selecciona los grupos de origen que proporcionan los tipos de sensores necesarios para tu escenario de aplicación. Para este ejemplo, simplemente tenemos un grupo de origen que proporciona fotogramas desde una cámara RGB.

[!code-cs[OpenCVFrameSourceGroups](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameSourceGroups)]

## <a name="initialize-the-mediacapture-object"></a>Inicializar el objeto MediaCapture
Después, debes inicializar el objeto **MediaCapture** para usar el grupo de origen de fotogramas seleccionado en el paso anterior configurando la propiedad **[SourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)** de **MediaCaptureInitializationSettings**.

> [!NOTE] 
> La técnica usada por el componente OpenCVHelper, que se describe con detalle en [Procesar mapas de bits de software con OpenCV](process-software-bitmaps-with-opencv.md), requiere que los datos de imagen residan en la memoria CPU, no en la memoria GPU. Por lo tanto, debes especificar **MemoryPreference.CPU** para el campo **[MemoryPreference](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.MemoryPreference)** de **MediaCaptureInitializationSettings**.

Una vez que el objeto **MediaCapture** se ha inicializado, obtén una referencia al origen de fotogramas RGB accediendo a la propiedad **[MediaCapture.FrameSources](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.FrameSources)** .

[!code-cs[OpenCVInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVInitMediaCapture)]

## <a name="initialize-the-mediaframereader"></a>Inicializar MediaFrameReader
Luego crea [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) para el origen de fotogramas RGB recuperado en el paso anterior. Para mantener una velocidad de fotogramas óptima, es posible que quieras procesar los fotogramas que tengan una resolución inferior a la resolución del sensor. En este ejemplo se proporciona el argumento opcional **[BitmapSize](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapsize)** al método **[MediaCapture.CreateFrameReaderAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync)** para solicitar que los fotogramas proporcionados por el lector de fotogramas cambien a 640 x 480 píxeles.

Después de crear el lector de fotogramas, registra un controlador para el evento **[FrameArrived](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.FrameArrived)** . Luego crea un nuevo objeto **[SoftwareBitmapSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource)** , que la clase auxiliar **FrameRenderer** usará para presentar la imagen procesada. Después llama al constructor para **FrameRenderer**. Initialize the instance of the **OpenCVHelper** class defined in the OpenCVBridge Windows Runtime component. Esta clase auxiliar se usa en el controlador **FrameArrived** para procesar cada fotograma. Por último, inicia el lector de fotogramas llamando a **[StartAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.StartAsync)** .

[!code-cs[OpenCVFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameReader)]


## <a name="handle-the-framearrived-event"></a>Controlar el evento FrameArrived
El evento **FrameArrived** se genera siempre que un nuevo fotograma esté disponible desde el lector de fotogramas. Llama a **[TryAcquireLatestFrame](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.TryAcquireLatestFrame)** para obtener el fotograma, si existe. Obtén **SoftwareBitmap** de **[MediaFrameReference](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereference)** . Ten en cuenta que la clase **CVHelper** usada en este ejemplo requiere imágenes para usar el formato de píxeles BRGA8 con valores alfa multiplicados previamente. Si el fotograma que pasó al evento tiene un formato distinto, convierte **SoftwareBitmap** en el formato correcto. Luego crea un **SoftwareBitmap** para usarse como el destino de la operación de desenfoque. Las propiedades de imagen de origen se usan como argumentos para que el constructor cree un mapa de bits con el formato que coincida. Llama al método **Desenfoque** de clase auxiliar para procesar el fotograma. Por último, pasa la imagen de salida de la operación de desenfoque a **PresentSoftwareBitmap**, el método de la clase auxiliar **FrameRenderer** que muestra la imagen en el control **Image** XAML con el que se inicializó.

[!code-cs[OpenCVFrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameArrived)]

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Basic photo, video, and audio capture with MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Process media frames with MediaFrameReader](process-media-frames-with-mediaframereader.md)
* [Process software bitmaps with OpenCV](process-software-bitmaps-with-opencv.md)
* [Camera frames sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [Camera Frames + OpenCV Sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV)
 

 




