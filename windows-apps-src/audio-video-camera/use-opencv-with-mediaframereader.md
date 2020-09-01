---
ms.assetid: ''
description: En este artículo se muestra cómo usar la biblioteca de Computer Vision de código abierto (OpenCV) con la clase MediaFrameReader.
title: Usar OpenCV con MediaFrameReader
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, openCV
ms.localizationpriority: medium
ms.openlocfilehash: 2128313c48f8d279a23cf63278b3e853c00348e4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175659"
---
# <a name="use-the-open-source-computer-vision-library-opencv-with-mediaframereader"></a>Usar la biblioteca de Computer Vision de código abierto (OpenCV) con MediaFrameReader

En este artículo se muestra cómo usar la biblioteca de Computer Vision de código abierto (OpenCV), una biblioteca de código nativo que proporciona una amplia variedad de algoritmos de procesamiento de imágenes, con la clase [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) que puede leer fotogramas multimedia de varios orígenes simulataneously. El código de ejemplo de este artículo le guía a través de la creación de una aplicación sencilla que obtiene fotogramas de un sensor de color, desenfoca cada fotograma mediante la biblioteca OpenCV y, a continuación, muestra la imagen procesada en un control de **imagen** XAML. 

>[!NOTE]
>OpenCV. win. Core y OpenCV. win. ImgProc no se actualizan con regularidad y no superan las comprobaciones de cumplimiento del almacén, por lo que estos paquetes solo están diseñados para la experimentación.

Este artículo se basa en el contenido de otros dos artículos:

* [Procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md) : en este artículo se proporciona información detallada sobre el uso de **MediaFrameReader** para obtener fotogramas de uno o más orígenes de fotogramas multimedia y se describe, en detalle, la mayor parte del código de ejemplo de este artículo. En concreto, **procesar fotogramas multimedia con MediaFrameReader** proporciona la lista de código para una clase auxiliar, **FrameRenderer**, que controla la presentación de los fotogramas multimedia en un elemento de **imagen** XAML. En el código de ejemplo de este artículo se usa también esta clase auxiliar.

* [Procesar mapas de bits de software con OpenCV](process-software-bitmaps-with-opencv.md) : este artículo le guía a través de la creación de un componente de Windows Runtime de código nativo, **OpenCVBridge**, que ayuda a realizar la conversión entre el objeto **SoftwareBitmap** , usado por **MediaFrameReader**, y el tipo de **paspartú** usado por la biblioteca de OpenCV. En el código de ejemplo de este artículo se supone que ha seguido los pasos para agregar el componente **OpenCVBridge** a la solución de aplicación para UWP.

Además de estos artículos, para ver y descargar un ejemplo completo de trabajo de un extremo a otro del escenario que se describe en este artículo, consulte el [ejemplo Camera frames + OpenCV](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV) en el repositorio de github de ejemplos de Windows universal.

Para empezar a desarrollar rápidamente, puede incluir la biblioteca OpenCV en un proyecto de aplicación para UWP mediante el uso de paquetes NuGet, pero es posible que estos paquetes no pasen el proceso de certficication de la aplicación al enviar la aplicación a la tienda, por lo que se recomienda descargar el código fuente de la biblioteca OpenCV y compilar los archivos binarios usted mismo antes de enviar la aplicación. Puede encontrar información sobre el desarrollo con OpenCV en [https://opencv.org](https://opencv.org)


## <a name="implement-the-opencvhelper-native-windows-runtime-component"></a>Implementar el componente de Windows Runtime nativo de OpenCVHelper
Siga los pasos de [proceso de mapas de bits de software con OpenCV](process-software-bitmaps-with-opencv.md) para crear el componente auxiliar de OpenCV Windows Runtime y agregue una referencia al proyecto de componente a la solución de aplicación para UWP.

## <a name="find-available-frame-source-groups"></a>Buscar grupos de orígenes de fotogramas disponibles
En primer lugar, debe buscar un grupo de origen de fotogramas multimedia del que se obtendrán los fotogramas multimedia. Obtiene la lista de grupos de origen disponibles en el dispositivo actual llamando a **[MediaFrameSourceGroup. FindAllAsync](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)**. Después, seleccione los grupos de orígenes que proporcionan los tipos de sensor necesarios para el escenario de la aplicación. En este ejemplo, simplemente necesitamos un grupo de origen que proporcione fotogramas de una cámara RGB.

[!code-cs[OpenCVFrameSourceGroups](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameSourceGroups)]

## <a name="initialize-the-mediacapture-object"></a>Inicializar el objeto MediaCapture
A continuación, debe inicializar el objeto **MediaCapture** para usar el grupo de origen del marco seleccionado en el paso anterior estableciendo la propiedad **[SourceGroup](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)** de **MediaCaptureInitializationSettings**.

> [!NOTE] 
> La técnica usada por el componente OpenCVHelper, que se describe en detalle en el [proceso mapas de bits de software con OpenCV](process-software-bitmaps-with-opencv.md), requiere que los datos de la imagen residan en la memoria de la CPU, no en la memoria GPU. Por lo tanto, debe especificar **MemoryPreference. CPU** para el campo **[MemoryPreference](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.MemoryPreference)** de **MediaCaptureInitializationSettings**.

Una vez inicializado el objetos **mediacapture** , obtenga una referencia al origen de fotogramas RGB mediante el acceso a la propiedad **[MediaCapture. FrameSources](/uwp/api/windows.media.capture.mediacapture.FrameSources)** .

[!code-cs[OpenCVInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVInitMediaCapture)]

## <a name="initialize-the-mediaframereader"></a>Inicializar MediaFrameReader
A continuación, cree un [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) para el origen de fotogramas RGB recuperado en el paso anterior. Con el fin de mantener una buena velocidad de fotogramas, es posible que desee procesar fotogramas que tengan una resolución inferior a la resolución del sensor. En este ejemplo se proporciona el argumento **[BitmapSize](/uwp/api/windows.graphics.imaging.bitmapsize)** opcional al método **[MediaCapture. CreateFrameReaderAsync](/uwp/api/windows.media.capture.mediacapture.createframereaderasync)** para solicitar que se cambie el tamaño de los fotogramas proporcionados por el lector de fotogramas a 640 x 480 píxeles.

Después de crear el lector de fotogramas, registre un controlador para el evento **[FrameArrived](/uwp/api/windows.media.capture.frames.mediaframereader.FrameArrived)** . A continuación, cree un nuevo objeto **[SoftwareBitmapSource](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource)** , que usará la clase auxiliar **FrameRenderer** para presentar la imagen procesada. A continuación, llame al constructor para **FrameRenderer**. Inicialice la instancia de la clase **OpenCVHelper** definida en el componente de Windows Runtime OpenCVBridge. Esta clase auxiliar se usa en el controlador **FrameArrived** para procesar cada fotograma. Por último, inicie el lector de fotogramas llamando a **[StartAsync](/uwp/api/windows.media.capture.frames.mediaframereader.StartAsync)**.

[!code-cs[OpenCVFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameReader)]


## <a name="handle-the-framearrived-event"></a>Controlar el evento FrameArrived
El evento **FrameArrived** se desencadena cuando un nuevo marco está disponible en el lector de fotogramas. Llame a **[TryAcquireLatestFrame](/uwp/api/windows.media.capture.frames.mediaframereader.TryAcquireLatestFrame)** para obtener el fotograma, si existe. Obtiene el **SoftwareBitmap** desde **[MediaFrameReference](/uwp/api/windows.media.capture.frames.mediaframereference)**. Tenga en cuenta que la clase **CVHelper** que se usa en este ejemplo requiere que las imágenes usen el formato de píxel BRGA8 con alfa premultiplicado. Si el marco que se pasa al evento tiene un formato diferente, convierta el **SoftwareBitmap** en el formato correcto. A continuación, cree un **SoftwareBitmap** que se usará como destino de la operación de desenfoque. Las propiedades de la imagen de origen se usan como argumentos en el constructor para crear un mapa de bits con formato coincidente. Llame al método **Blur** de la clase auxiliar para procesar el marco. Por último, pase la imagen de salida de la operación de desenfoque a **PresentSoftwareBitmap**, el método de la clase auxiliar **FrameRenderer** que muestra la imagen en el control de **imagen** XAML con el que se inicializó.

[!code-cs[OpenCVFrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameArrived)]

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md)
* [Procesamiento de mapas de bits de software con OpenCV](process-software-bitmaps-with-opencv.md)
* [Muestra de fotogramas de cámara](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [Fotogramas de cámara + ejemplo de OpenCV](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV)
 

 