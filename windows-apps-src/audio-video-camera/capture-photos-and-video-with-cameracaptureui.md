---
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: En este artículo se describe cómo usar la clase CameraCaptureUI para capturar fotos o vídeos con la interfaz de usuario de la cámara integrada en Windows.
title: Capturar fotografías y vídeos con la interfaz de usuario de la cámara integrada de Windows
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 35eed8310b406a960334c90d6c359c0313b2660c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358897"
---
# <a name="capture-photos-and-video-with-windows-built-in-camera-ui"></a>Capturar fotografías y vídeos con la interfaz de usuario de la cámara integrada de Windows



En este artículo se describe cómo usar la clase CameraCaptureUI para capturar fotos o vídeos con la interfaz de usuario de la cámara integrada en Windows. Esta característica es fácil de usar y permite a la aplicación obtener una foto o un vídeo capturado por el usuario con tan solo unas pocas líneas de código.

Si deseas proporcionar tu propia interfaz de usuario de la cámara o si tu escenario requiere un control de bajo nivel más sólido de la operación de captura, debes usar el objeto [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) e implementar tu propia experiencia de captura. Para obtener más información, consulta [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

> [!NOTE]
> No se debe especificar el **webcam** o **micrófono** capacidades en su aplicación el archivo de manifiesto si la aplicación solo usa CameraCaptureUI. Si lo haces, la aplicación se mostrará en la configuración de privacidad de la cámara del dispositivo, pero aunque el usuario deniegue el acceso de la cámara a tu aplicación, no impedirá que CameraCaptureUI capture multimedia. Esto es porque la aplicación de cámara integrada de Windows es una aplicación de origen de confianza que requiere que el usuario inicie la captura de fotos, audio y vídeo con la presión de un botón. La aplicación puede producir un error de certificación del Kit de certificación de aplicaciones de Windows cuando envía a la Store si especifica las capacidades de cámara Web o micrófono cuando se usa CameraCaptureUI como su único mecanismo de captura de fotos.
> Debes especificar las funcionalidades de cámara web o micrófono en el archivo de manifiesto de la aplicación si usas MediaCapture para capturar audio, fotos o vídeo mediante programación.

## <a name="capture-a-photo-with-cameracaptureui"></a>Capturar una foto con CameraCaptureUI

Para usar la interfaz de usuario de captura de cámara, incluye el espacio de nombres [**Windows.Media.Capture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture) en el proyecto. Para realizar operaciones de archivo con el archivo de imagen devuelto, incluye [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage).

[!code-cs[UsingCaptureUI](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingCaptureUI)]

Para capturar una foto, crea un objeto [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) nuevo. El uso de la propiedad [**PhotoSettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.photosettings) del objeto puede especificar las propiedades de la foto devuelta, como el formato de imagen de la foto. De manera predeterminada, la interfaz de usuario de captura de cámara permite al usuario recortar la foto antes de que se devuelva, aunque esto se puede deshabilitar con la propiedad [**AllowCropping**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping). Este ejemplo establece [**CroppedSizeInPixels**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.croppedsizeinpixels) para solicitar que la imagen devuelta sea de 200 x 200 píxeles.

> [!NOTE]
> El recorte de imágenes en **CameraCaptureUI** no se admite en los dispositivos de la familia de dispositivos móviles. El valor de la propiedad [**AllowCropping**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) se omite cuando se ejecuta la aplicación en estos dispositivos.

Realiza una llamada a [**CaptureFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.) y especifica [**CameraCaptureUIMode.Photo**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUIMode) para especificar que se debe capturar una foto. El método devuelve una instancia [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) que contiene la imagen si la captura se realiza correctamente. Si el usuario cancela la captura, el objeto devuelto es null.

[!code-cs[CapturePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCapturePhoto)]

A la clase **StorageFile** que contiene la foto capturada se le asigna un nombre generado dinámicamente y se guarda en la carpeta local de la aplicación. Para organizar mejor las fotos capturadas, puedes que desees mover el archivo a otra carpeta.

[!code-cs[CopyAndDeletePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCopyAndDeletePhoto)]

Para usar la foto en la aplicación, tal vez quieras crear un objeto [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) que se puede usar con varias características diferentes de una aplicación universal de Windows.

Primero, debes incluir el espacio de nombres [**Windows.Graphics.Imaging**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging) en el proyecto.

[!code-cs[UsingSoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmap)]

Realiza una llamada a [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.istoragefile.openasync) para obtener una secuencia del archivo de imagen. Realiza una llamada a [**BitmapDecoder.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) para obtener un descodificador de mapa de bits de la secuencia. A continuación, realiza una llamada a [**GetSoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) para obtener una representación **SoftwareBitmap** de la imagen.

[!code-cs[SoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmap)]

Para mostrar la imagen en la interfaz de usuario, declara un control de [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) en la página XAML.

[!code-xml[ImageControl](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetImageControl)]

Para usar el mapa de bits de software en tu página XAML, incluye el espacio de nombres [**Windows.UI.Xaml.Media.Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging) de "using" en el proyecto.

[!code-cs[UsingSoftwareBitmapSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmapSource)]

El control de **Image** requiere que el origen de imagen tenga el formato BGRA8 con valores alfa premultiplicados o sin alfa. Por lo tanto, realiza una llamada al método estático [**SoftwareBitmap.Convert**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.windows) para crear un nuevo mapa de bits de software con el formato deseado. A continuación, crea un objeto [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) nuevo y realiza una llamada a [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) para asignar el mapa de bits de software al origen. Por último, establece el control de **Image** de la propiedad [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) del control para mostrar la foto capturada en la interfaz de usuario.

[!code-cs[SetImageSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetImageSource)]

## <a name="capture-a-video-with-cameracaptureui"></a>Capturar un vídeo con CameraCaptureUI

Para capturar un vídeo, crea un objeto [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) nuevo. El uso de la propiedad [**VideoSettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.videosettings) del objeto puede especificar las propiedades del vídeo devuelto, como el formato de imagen del vídeo.

Realiza una llamada a [**CaptureFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.) y especifica [**Video**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.videosettings) para especificar que se debe capturar un vídeo. El método devuelve una instancia [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) que contiene el vídeo si la captura se realiza correctamente. Si el usuario cancela la captura, el objeto devuelto es null.

[!code-cs[CaptureVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCaptureVideo)]

Qué hacer con el archivo de vídeo capturado depende del escenario para la aplicación. El resto de este artículo muestra cómo crear rápidamente una composición multimedia de uno o más vídeos capturados y mostrarla en la interfaz de usuario.

En primer lugar, agrega un control [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) en el que se mostrará la composición de vídeo en la página XAML.

[!code-xml[MediaElement](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetMediaElement)]


Con el archivo de vídeo devuelto desde la interfaz de usuario de captura de cámara, crea una clase [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) nueva llamando a **[CreateFromStorageFile](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstoragefile)** . Llama al método **[Play](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Play)** del **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** predeterminado asociado al **MediaPlayerElement** para reproducir el vídeo.

[!code-cs[PlayVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetPlayVideo)]
 

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Capturar básica de fotos, vídeo y audio con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 
 

 




