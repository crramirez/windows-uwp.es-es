---
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: En este artículo se describe cómo usar la clase CameraCaptureUI para capturar fotografías o vídeos mediante la interfaz de usuario de la cámara integrada en Windows.
title: Capturar fotos y vídeos con la interfaz de usuario de la cámara integrada de Windows
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d582d4815b4fb2168b187a1efff3795cc98aca02
ms.sourcegitcommit: 99595e4938213aafdb49635d684d8ba8eb3f697a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69487803"
---
# <a name="capture-photos-and-video-with-the-windows-built-in-camera-ui"></a>Capturar fotos y vídeos con la interfaz de usuario de la cámara integrada de Windows



En este artículo se describe cómo usar la clase CameraCaptureUI para capturar fotografías o vídeos mediante la interfaz de usuario de la cámara integrada en Windows. Esta característica es fácil de usar. Permite que la aplicación obtenga una foto o un vídeo capturado por el usuario con solo unas pocas líneas de código.

Si deseas proporcionar tu propia interfaz de usuario de la cámara o si tu escenario requiere un control de bajo nivel más sólido de la operación de captura, debes usar el objeto [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) e implementar tu propia experiencia de captura. Para obtener más información, consulta [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

> [!NOTE]
> No debe especificar las funcionalidades de **cámara web** o **micrófono** en el archivo de manifiesto de la aplicación si la aplicación solo usa CameraCaptureUI. Si lo hace, la aplicación se mostrará en la configuración de privacidad de la cámara del dispositivo, pero incluso si el usuario deniega el acceso a la cámara a la aplicación, esto no impedirá que el CameraCaptureUI Capture los medios. <p>Esto es porque la aplicación de cámara integrada de Windows es una aplicación de origen de confianza que requiere que el usuario inicie la captura de fotos, audio y vídeo con la presión de un botón. La aplicación puede producir un error de certificación del kit de certificación de aplicaciones de Windows cuando se envía a Microsoft Store si especifica las funcionalidades de cámara web o micrófono al usar CameraCaptureUI como el único mecanismo de captura de fotos.<p>
Debe especificar las capacidades de cámara web o micrófono en el archivo de manifiesto de la aplicación si está usando MediaCapture para capturar audio, fotos o vídeo mediante programación.

## <a name="capture-a-photo-with-cameracaptureui"></a>Capturar una foto con CameraCaptureUI

Para usar la interfaz de usuario de captura de cámara, incluye el espacio de nombres [**Windows.Media.Capture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture) en el proyecto. Para realizar operaciones de archivo con el archivo de imagen devuelto, incluye [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage).

[!code-cs[UsingCaptureUI](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingCaptureUI)]

Para capturar una foto, crea un objeto [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) nuevo. Mediante el uso de la propiedad [**PhotoSettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.photosettings) del objeto, puede especificar las propiedades de la foto devuelta, como el formato de imagen de la fotografía. De forma predeterminada, la interfaz de usuario de captura de cámara permite recortar la foto antes de que se devuelva. Se puede deshabilitar con la propiedad [**AllowCropping**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) . Este ejemplo establece [**CroppedSizeInPixels**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.croppedsizeinpixels) para solicitar que la imagen devuelta sea de 200 x 200 píxeles.

> [!NOTE]
> El recorte de imagen en **CameraCaptureUI** no es compatible con dispositivos de la familia de dispositivos móviles. El valor de la propiedad [**AllowCropping**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) se omite cuando se ejecuta la aplicación en estos dispositivos.

Realiza una llamada a [**CaptureFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) y especifica [**CameraCaptureUIMode.Photo**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUIMode) para especificar que se debe capturar una foto. El método devuelve una instancia [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) que contiene la imagen si la captura se realiza correctamente. Si el usuario cancela la captura, el objeto devuelto es null.

[!code-cs[CapturePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCapturePhoto)]

A la clase **StorageFile** que contiene la foto capturada se le asigna un nombre generado dinámicamente y se guarda en la carpeta local de la aplicación. Para organizar mejor las fotos capturadas, puede trasladar el archivo a una carpeta diferente.

[!code-cs[CopyAndDeletePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCopyAndDeletePhoto)]

Para usar la foto en la aplicación, tal vez quieras crear un objeto [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) que se puede usar con varias características diferentes de una aplicación universal de Windows.

En primer lugar, incluya el espacio de nombres [**Windows. Graphics. Imaging**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging) en el proyecto.

[!code-cs[UsingSoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmap)]

Realiza una llamada a [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.istoragefile.openasync) para obtener una secuencia del archivo de imagen. Realiza una llamada a [**BitmapDecoder.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) para obtener un descodificador de mapa de bits de la secuencia. A continuación, llame a [**GetSoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) para obtener una representación de **SoftwareBitmap** de la imagen.

[!code-cs[SoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmap)]

Para mostrar la imagen en la interfaz de usuario, declara un control de [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) en la página XAML.

[!code-xml[ImageControl](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetImageControl)]

Para usar el mapa de bits de software en tu página XAML, incluye el espacio de nombres [**Windows.UI.Xaml.Media.Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging) de "using" en el proyecto.

[!code-cs[UsingSoftwareBitmapSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmapSource)]

El control de **imagen** requiere que el origen de la imagen tenga el formato BGRA8 con alfa premultiplicado o sin alfa. Llame al método estático [**SoftwareBitmap. Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) para crear un nuevo mapa de bits de software con el formato deseado. A continuación, cree un nuevo objeto [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) y llámelo [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) para asignar el mapa de bits de software al origen. Por último, establece el control de **Image** de la propiedad [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) del control para mostrar la foto capturada en la interfaz de usuario.

[!code-cs[SetImageSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetImageSource)]

## <a name="capture-a-video-with-cameracaptureui"></a>Capturar un vídeo con CameraCaptureUI

Para capturar un vídeo, crea un objeto [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) nuevo. Mediante el uso de la propiedad [**VideoSettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.videosettings) del objeto, puede especificar las propiedades del vídeo devuelto, como el formato del vídeo.

Llame a [**CaptureFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) y especifique el [**vídeo**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.videosettings) para capturar un vídeo. El método devuelve una instancia [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) que contiene el vídeo si la captura se realiza correctamente. Si cancela la captura, el objeto devuelto es NULL.

[!code-cs[CaptureVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCaptureVideo)]

Qué hacer con el archivo de vídeo capturado depende del escenario para la aplicación. El resto de este artículo muestra cómo crear rápidamente una composición multimedia de uno o más vídeos capturados y mostrarla en la interfaz de usuario.

En primer lugar, agregue un control [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) en el que la composición de vídeo se mostrará en la página XAML.

[!code-xml[MediaElement](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetMediaElement)]


Cuando el archivo de vídeo vuelva de la interfaz de usuario de captura de cámara, cree un nuevo [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) llamando a **[CreateFromStorageFile](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstoragefile)** . Llama al método **[Play](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Play)** del **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** predeterminado asociado al **MediaPlayerElement** para reproducir el vídeo.

[!code-cs[PlayVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetPlayVideo)]
 

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, vídeo y audio con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 
 

 




