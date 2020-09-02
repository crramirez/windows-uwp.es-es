---
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: En este artículo se describe cómo usar la clase [**CameraCaptureUI**](/uwp/api/windows.media.capture.cameracaptureui) para capturar fotografías o vídeos mediante la interfaz de usuario de la cámara integrada en Windows.
title: Capturar fotografías y vídeos con la interfaz de usuario de la cámara integrada de Windows
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 5b5e1369e37fc683a3a09c8f404b1998ee06bdab
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364008"
---
# <a name="capture-photos-and-video-with-the-windows-built-in-camera-ui"></a>Capturar fotografías y vídeos con la interfaz de usuario de la cámara integrada de Windows

En este artículo se describe cómo usar la clase [**CameraCaptureUI**](/uwp/api/windows.media.capture.cameracaptureui) para capturar fotografías o vídeos mediante la interfaz de usuario de la cámara integrada en Windows. Esta característica es fácil de usar. Permite que la aplicación obtenga una foto o un vídeo capturado por el usuario con solo unas pocas líneas de código.

Si desea proporcionar su propia interfaz de usuario de la cámara, o si su escenario requiere un control más sólido y de bajo nivel de la operación de captura, debe usar la clase [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) e implementar su propia experiencia de captura. Para obtener más información, consulta [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

> [!NOTE]
> No debe especificar las funcionalidades de la **cámara web** ni del **micrófono** en el archivo de manifiesto de la aplicación si la aplicación solo usa **CameraCaptureUI**. Si lo hace, la aplicación se mostrará en la configuración de privacidad de la cámara del dispositivo, pero incluso si el usuario deniega el acceso a la cámara a la aplicación, esto no impedirá que el **CameraCaptureUI** Capture los medios. <p>Esto es porque la aplicación de cámara integrada de Windows es una aplicación de origen de confianza que requiere que el usuario inicie la captura de fotos, audio y vídeo con la presión de un botón. La aplicación puede producir un error de certificación del kit de certificación de aplicaciones de Windows cuando se envía a Microsoft Store si especifica las funcionalidades de cámara web o micrófono al usar **CameraCaptureUI** como el único mecanismo de captura de fotos.<p>
Debe especificar las capacidades de **cámara web** o **micrófono** en el archivo de manifiesto de la aplicación si está usando **MediaCapture** para capturar audio, fotos o vídeo mediante programación.

## <a name="capture-a-photo-with-cameracaptureui"></a>Capturar una foto con CameraCaptureUI

Para usar la interfaz de usuario de captura de cámara, incluye el espacio de nombres [**Windows.Media.Capture**](/uwp/api/Windows.Media.Capture) en el proyecto. Para realizar operaciones de archivo con el archivo de imagen devuelto, incluye [**Windows.Storage**](/uwp/api/Windows.Storage).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetUsingCaptureUI":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.h" id="SnippetUsingCaptureUI":::

Para capturar una foto, crea un objeto [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI) nuevo. Mediante el uso de la propiedad [**Fotosettings**](/uwp/api/windows.media.capture.cameracaptureui.photosettings) del objeto, puede especificar las propiedades de la foto devuelta, como el formato de imagen de la fotografía. De forma predeterminada, la interfaz de usuario de captura de cámara permite recortar la foto antes de que se devuelva. Se puede deshabilitar con la propiedad [**AllowCropping**](/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) . Este ejemplo establece [**CroppedSizeInPixels**](/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.croppedsizeinpixels) para solicitar que la imagen devuelta sea de 200 x 200 píxeles.

> [!NOTE]
> El recorte de imagen en **CameraCaptureUI** no es compatible con dispositivos de la familia de dispositivos móviles. El valor de la propiedad [**AllowCropping**](/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) se omite cuando se ejecuta la aplicación en estos dispositivos.

Realiza una llamada a [**CaptureFileAsync**](/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) y especifica [**CameraCaptureUIMode.Photo**](/uwp/api/Windows.Media.Capture.CameraCaptureUIMode) para especificar que se debe capturar una foto. El método devuelve una instancia [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) que contiene la imagen si la captura se realiza correctamente. Si el usuario cancela la captura, el objeto devuelto es null.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetCapturePhoto":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetCapturePhoto":::

A la clase **StorageFile** que contiene la foto capturada se le asigna un nombre generado dinámicamente y se guarda en la carpeta local de la aplicación. Para organizar mejor las fotos capturadas, puede trasladar el archivo a una carpeta diferente.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetCopyAndDeletePhoto":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetCopyAndDeletePhoto":::

Para usar la foto en la aplicación, tal vez quieras crear un objeto [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) que se puede usar con varias características diferentes de una aplicación universal de Windows.

En primer lugar, incluya el espacio de nombres [**Windows. Graphics. Imaging**](/uwp/api/Windows.Graphics.Imaging) en el proyecto.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetUsingSoftwareBitmap":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.h" id="SnippetUsingSoftwareBitmap":::

Realiza una llamada a [**OpenAsync**](/uwp/api/windows.storage.istoragefile.openasync) para obtener una secuencia del archivo de imagen. Realiza una llamada a [**BitmapDecoder.CreateAsync**](/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) para obtener un descodificador de mapa de bits de la secuencia. A continuación, llame a [**GetSoftwareBitmap**](/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) para obtener una representación de **SoftwareBitmap** de la imagen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetSoftwareBitmap":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetSoftwareBitmap":::

Para mostrar la imagen en la interfaz de usuario, declara un control de [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) en la página XAML.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml" id="SnippetImageControl":::
:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.xaml" id="SnippetImageControl":::

Para usar el mapa de bits de software en tu página XAML, incluye el espacio de nombres [**Windows.UI.Xaml.Media.Imaging**](/uwp/api/Windows.UI.Xaml.Media.Imaging) de "using" en el proyecto.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetUsingSoftwareBitmapSource":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.h" id="SnippetUsingSoftwareBitmapSource":::

El control de **imagen** requiere que el origen de la imagen tenga el formato BGRA8 con alfa premultiplicado o sin alfa. Llame al método estático [**SoftwareBitmap. Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) para crear un nuevo mapa de bits de software con el formato deseado. A continuación, cree un nuevo objeto [**SoftwareBitmapSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) y llámelo [**SetBitmapAsync**](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) para asignar el mapa de bits de software al origen. Por último, establece el control de **Image** de la propiedad [**Source**](/uwp/api/windows.ui.xaml.controls.image.source) del control para mostrar la foto capturada en la interfaz de usuario.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetSetImageSource":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetSetImageSource":::

## <a name="capture-a-video-with-cameracaptureui"></a>Capturar un vídeo con CameraCaptureUI

Para capturar un vídeo, crea un objeto [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI) nuevo. Mediante el uso de la propiedad [**Videosettings**](/uwp/api/windows.media.capture.cameracaptureui.videosettings) del objeto, puede especificar las propiedades del vídeo devuelto, como el formato del vídeo.

Llame a [**CaptureFileAsync**](/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) y especifique el [**vídeo**](/uwp/api/windows.media.capture.cameracaptureui.videosettings) para capturar un vídeo. El método devuelve una instancia [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) que contiene el vídeo si la captura se realiza correctamente. Si cancela la captura, el objeto devuelto es NULL.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetCaptureVideo":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetCaptureVideo":::

Qué hacer con el archivo de vídeo capturado depende del escenario para la aplicación. El resto de este artículo muestra cómo crear rápidamente una composición multimedia de uno o más vídeos capturados y mostrarla en la interfaz de usuario.

En primer lugar, agregue un control [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) en el que la composición de vídeo se mostrará en la página XAML.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml" id="SnippetMediaElement":::

Cuando el archivo de vídeo vuelva de la interfaz de usuario de captura de cámara, cree un nuevo [**MediaSource**](/uwp/api/windows.media.core.mediasource) llamando a **[CreateFromStorageFile](/uwp/api/windows.media.core.mediasource.createfromstoragefile)**. Llame al método **[Play](/uwp/api/windows.media.playback.mediaplayer.Play)** del objeto **[MediaPlayer](/uwp/api/windows.media.playback.mediaplayer)** predeterminado asociado a la clase **MediaPlayerElement** para reproducir el vídeo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetPlayVideo":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetPlayVideo":::

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [CameraCaptureUI](/uwp/api/Windows.Media.Capture.CameraCaptureUI)
