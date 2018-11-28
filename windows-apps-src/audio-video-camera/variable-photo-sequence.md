---
ms.assetid: 7DBEE5E2-C3EC-4305-823D-9095C761A1CD
description: En este artículo se muestra cómo capturar una secuencia de fotos variable, que permite capturar varios fotogramas de imágenes en sucesión rápida y configurar cada uno para que use una configuración de foco, flash, ISO, exposición y compensación de la exposición diferente.
title: Secuencia de fotos variable
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 208a61b565c0522d3e9ce88f3938f57dfa1fbddd
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7971276"
---
# <a name="variable-photo-sequence"></a>Secuencia de fotos variable



En este artículo se muestra cómo capturar una secuencia de fotos variable, que permite capturar varios fotogramas de imágenes en sucesión rápida y configurar cada uno para que use una configuración de foco, flash, ISO, exposición y compensación de la exposición diferente. Esta característica permite escenarios como crear imágenes de alto rango dinámico (HDR).

Si deseas capturar imágenes HDR pero no quieres implementar tu propio algoritmo de procesamiento, puedes usar la API [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) para usar las funcionalidades HDR integradas en Windows. Para obtener más información, consulta [Captura de fotos de alto rango dinámico (HDR)](high-dynamic-range-hdr-photo-capture.md).

> [!NOTE] 
> Este artículo se basa en los conceptos y el código analizados en [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), donde se describen los pasos para implementar la captura básica de fotos y vídeo. Se recomienda que te familiarices con el patrón de captura de multimedia básico de ese artículo antes de pasar a escenarios más avanzados de captura. El código que encontrarás en este artículo se ha agregado suponiendo que la aplicación ya tiene una instancia de MediaCapture inicializada correctamente.

## <a name="set-up-your-app-to-use-variable-photo-sequence-capture"></a>Configurar la aplicación para usar la captura de secuencia de fotos variable

Además de los espacios de nombres necesarios para la captura de multimedia básico, la implementación de una captura de secuencia fotográfica variable requiere los siguientes espacios de nombres.

[!code-cs[VPSUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSUsing)]

Declara una variable de miembro para almacenar el objeto [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564), que se usa para iniciar la captura de secuencia de fotos. Declara una matriz de objetos [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) para almacenar cada imagen capturada en la secuencia. Además, declara una matriz para almacenar el objeto [**CapturedFrameControlValues**](https://msdn.microsoft.com/library/windows/apps/dn608020) por cada fotograma. Puede usarse por su algoritmo de procesamiento de imagen para determinar qué configuración se usó para capturar cada fotograma. Por último, declara un índice que se usará para dar seguimiento a la imagen en la secuencia que se está capturando en el momento.

[!code-cs[VPSMemberVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSMemberVariables)]

## <a name="prepare-the-variable-photo-sequence-capture"></a>Preparar la captura de secuencia de fotos variable

Después de inicializar tu objeto [MediaCapture](capture-photos-and-video-with-mediacapture.md), asegúrate de que las secuencias de fotos variables son compatibles con el dispositivo actual. Para ello, obtén una instancia del objeto [**VariablePhotoSequenceController**](https://msdn.microsoft.com/library/windows/apps/dn640573) de la captura multimedia [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) y comprueba la propiedad [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn640580).

[!code-cs[IsVPSSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsVPSSupported)]

Obtén un objeto [**FrameControlCapabilities**](https://msdn.microsoft.com/library/windows/apps/dn652548) desde el controlador de secuencia fotográfica variable. Este objeto tiene una propiedad para cada configuración que pueda configurarse por fotograma de una secuencia de fotos. Estas son:

-   [**Exposure**](https://msdn.microsoft.com/library/windows/apps/dn652552)
-   [**ExposureCompensation**](https://msdn.microsoft.com/library/windows/apps/dn652560)
-   [**Flash**](https://msdn.microsoft.com/library/windows/apps/dn652566)
-   [**Focus**](https://msdn.microsoft.com/library/windows/apps/dn652570)
-   [**IsoSpeed**](https://msdn.microsoft.com/library/windows/apps/dn652574)
-   [**PhotoConfirmation**](https://msdn.microsoft.com/library/windows/apps/dn652578)

En este ejemplo se establece un valor de compensación de la exposición diferente para cada fotograma. Para comprobar que la compensación de la exposición es compatible con secuencias de fotos en el dispositivo actual, comprueba la propiedad [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn278905) del objeto [**FrameExposureCompensationCapabilities**](https://msdn.microsoft.com/library/windows/apps/dn652628) al que se accede a través de la propiedad **ExposureCompensation**.

[!code-cs[IsExposureCompensationSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsExposureCompensationSupported)]

Crea un nuevo objeto [**FrameController**](https://msdn.microsoft.com/library/windows/apps/dn652582) por cada fotograma que quieras capturar. En este ejemplo se capturan tres fotogramas. Establece los valores para los controles que desees variar para cada fotograma. A continuación, borra la colección [**DesiredFrameControllers**](https://msdn.microsoft.com/library/windows/apps/dn640574) de **VariablePhotoSequenceController** y agrega el controlador de cada fotograma a la colección.

[!code-cs[InitFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitFrameControllers)]

Crea un objeto [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) para establecer la codificación que desees usar para las imágenes capturadas. Llama al método estático [**MediaCapture.PrepareVariablePhotoSequenceCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/dn608097) y pasa las propiedades de codificación. Este método devuelve un objeto [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564). Por último, registra controladores de eventos para los eventos [**PhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/dn652573) y [**Stopped**](https://msdn.microsoft.com/library/windows/apps/dn652585).

[!code-cs[PrepareVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPrepareVPS)]

## <a name="start-the-variable-photo-sequence-capture"></a>Inicia la captura de una secuencia de fotos variable

Para iniciar la captura de la secuencia de fotos variable, llama a [**VariablePhotoSequenceCapture.StartAsync**](https://msdn.microsoft.com/library/windows/apps/dn652577). Asegúrate de inicializar las matrices para almacenar las imágenes capturadas y los valores de control de fotograma, y establece el índice actual en 0. Establece la variable de estado de grabación de la aplicación y actualiza la interfaz de usuario para deshabilitar el inicio de otra captura mientras esta está en curso.

[!code-cs[StartVPSCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartVPSCapture)]

## <a name="receive-the-captured-frames"></a>Recibe los fotogramas capturados

El evento [**PhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/dn652573) se genera para cada fotograma capturado. Guarda los valores de control y la imagen capturada para el fotograma y, a continuación, incrementa el índice del fotograma actual. Este ejemplo muestra cómo obtener una representación [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) de cada fotograma. Para obtener más información acerca del uso de **SoftwareBitmap**, consulta [Imagen](imaging.md).

[!code-cs[OnPhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnPhotoCaptured)]

## <a name="handle-the-completion-of-the-variable-photo-sequence-capture"></a>Administra la finalización de la captura de secuencia de fotos variable

El evento [**Stopped**](https://msdn.microsoft.com/library/windows/apps/dn652585) se produce cuando todos los fotogramas de la secuencia han sido capturados. Actualizar el estado de grabación de la aplicación y actualizar la interfaz de usuario para permitir al usuario iniciar nuevas capturas. En este punto, puedes pasar las imágenes capturadas y los valores de control de fotograma al código de procesamiento de la imagen.

[!code-cs[OnStopped](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnStopped)]

## <a name="update-frame-controllers"></a>Actualiza los controladores de fotograma

Si deseas realizar otra captura de secuencia de fotos variable con una configuración diferente para cada fotograma, no necesitas reiniciar completamente **VariablePhotoSequenceCapture**. Puedes borrar la colección [**DesiredFrameControllers**](https://msdn.microsoft.com/library/windows/apps/dn640574) y agregar nuevos controladores de fotograma, o puedes modificar los valores del controlador de fotograma existente. El siguiente ejemplo comprueba el objeto [**FrameFlashCapabilities**](https://msdn.microsoft.com/library/windows/apps/dn652657) para comprobar que el dispositivo actual es compatible con el flash y la potencia del flash para los fotogramas de una secuencia de fotos variable. Si es así, se actualiza cada fotograma para habilitar el flash al 100% de energía. Los valores de compensación de exposición que anteriormente estaban establecidos por cada fotograma siguen activos.

[!code-cs[UpdateFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateFrameControllers)]

## <a name="clean-up-the-variable-photo-sequence-capture"></a>Limpia la captura de una secuencia de fotos variable

Cuando hayas terminado de capturar secuencias de fotos variables o la aplicación esté suspendida, limpia el objeto de secuencia de fotos variable con una llamada a [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/dn652569). Anula el registro de controladores de eventos del objeto y establécelo en null.

[!code-cs[CleanUpVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpVPS)]

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




