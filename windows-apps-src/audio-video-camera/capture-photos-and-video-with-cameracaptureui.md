---
author: drewbatgit
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: "En este artículo se describe cómo usar la clase CameraCaptureUI para capturar fotos o vídeos con la interfaz de usuario de la cámara integrada en Windows."
title: "Capturar fotos y vídeos con la interfaz de usuario de la cámara integrada en Windows"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 7ddd659bc32eb8328f993fba4bfd4f137ea99d0f
ms.lasthandoff: 02/07/2017

---

# <a name="capture-photos-and-video-with-windows-built-in-camera-ui"></a>Capturar fotos y vídeos con la interfaz de usuario de la cámara integrada en Windows

\[ Actualizado para las aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


En este artículo se describe cómo usar la clase CameraCaptureUI para capturar fotos o vídeos con la interfaz de usuario de la cámara integrada en Windows. Esta característica es fácil de usar y permite a la aplicación obtener una foto o un vídeo capturado por el usuario con tan solo unas pocas líneas de código.

Si deseas proporcionar tu propia interfaz de usuario de la cámara o si tu escenario requiere un control de bajo nivel más sólido de la operación de captura, debes usar el objeto [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) e implementar tu propia experiencia de captura. Para obtener más información, consulta [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

> [!NOTE]
> No debes especificar las funcionalidades de **cámara web** o **micrófono** en el archivo de manifiesto de la aplicación si estás usando CameraCaptureUI. Si lo haces, la aplicación se mostrará en la configuración de privacidad de la cámara del dispositivo, pero aunque el usuario deniegue el acceso de la cámara a tu aplicación, no impedirá que CameraCaptureUI capture multimedia. Esto es porque la aplicación de cámara integrada de Windows es una aplicación de origen de confianza que requiere que el usuario inicie la captura de fotos, audio y vídeo con la presión de un botón. Puede que la aplicación no pase la certificación WACK (Kit para la certificación de aplicación Windows) cuando se envíe a la Tienda si especificas las funcionalidades de cámara web o micrófono al usar CameraCaptureUI.
> Debes especificar las funcionalidades de cámara web o micrófono en el archivo de manifiesto de la aplicación si usas MediaCapture para capturar audio, fotos o vídeo mediante programación.

## <a name="capture-a-photo-with-cameracaptureui"></a>Capturar una foto con CameraCaptureUI

Para usar la interfaz de usuario de captura de cámara, incluye el espacio de nombres [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) en el proyecto. Para realizar operaciones de archivo con el archivo de imagen devuelto, incluye [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346).

[!code-cs[UsingCaptureUI](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingCaptureUI)]

Para capturar una foto, crea un objeto [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) nuevo. El uso de la propiedad [**PhotoSettings**](https://msdn.microsoft.com/library/windows/apps/br241058) del objeto puede especificar las propiedades de la foto devuelta, como el formato de imagen de la foto. De manera predeterminada, la interfaz de usuario de captura de cámara permite al usuario recortar la foto antes de que se devuelva, aunque esto se puede deshabilitar con la propiedad [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042). Este ejemplo establece [**CroppedSizeInPixels**](https://msdn.microsoft.com/library/windows/apps/br241044) para solicitar que la imagen devuelta sea de 200 x 200 píxeles.

> [!NOTE]
> El recorte de imágenes en **CameraCaptureUI** no se admite en los dispositivos de la familia de dispositivos móviles. El valor de la propiedad [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042) se omite cuando se ejecuta la aplicación en estos dispositivos.

Realiza una llamada a [**CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057) y especifica [**CameraCaptureUIMode.Photo**](https://msdn.microsoft.com/library/windows/apps/br241040) para especificar que se debe capturar una foto. El método devuelve una instancia [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que contiene la imagen si la captura se realiza correctamente. Si el usuario cancela la captura, el objeto devuelto es null.

[!code-cs[CapturePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCapturePhoto)]

A la clase **StorageFile** que contiene la foto capturada se le asigna un nombre generado dinámicamente y se guarda en la carpeta local de la aplicación. Para organizar mejor las fotos capturadas, puedes que desees mover el archivo a otra carpeta.

[!code-cs[CopyAndDeletePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCopyAndDeletePhoto)]

Para usar la foto en la aplicación, tal vez quieras crear un objeto [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) que se puede usar con varias características diferentes de una aplicación universal de Windows.

Primero, debes incluir el espacio de nombres [**Windows.Graphics.Imaging**](https://msdn.microsoft.com/library/windows/apps/br226400) en el proyecto.

[!code-cs[UsingSoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmap)]

Realiza una llamada a [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227116) para obtener una secuencia del archivo de imagen. Realiza una llamada a [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/br226182) para obtener un descodificador de mapa de bits de la secuencia. A continuación, realiza una llamada a [**GetSoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887332) para obtener una representación **SoftwareBitmap** de la imagen.

[!code-cs[SoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmap)]

Para mostrar la imagen en la interfaz de usuario, declara un control de [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752) en la página XAML.

[!code-xml[ImageControl](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetImageControl)]

Para usar el mapa de bits de software en tu página XAML, incluye el espacio de nombres [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258) de "using" en el proyecto.

[!code-cs[UsingSoftwareBitmapSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmapSource)]

El control de **Image** requiere que el origen de imagen tenga el formato BGRA8 con valores alfa premultiplicados o sin alfa. Por lo tanto, realiza una llamada al método estático [**SoftwareBitmap.Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362) para crear un nuevo mapa de bits de software con el formato deseado. A continuación, crea un objeto [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) nuevo y realiza una llamada a [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856) para asignar el mapa de bits de software al origen. Por último, establece el control de **Image** de la propiedad [**Source**](https://msdn.microsoft.com/library/windows/apps/br242760) del control para mostrar la foto capturada en la interfaz de usuario.

[!code-cs[SetImageSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetImageSource)]

## <a name="capture-a-video-with-cameracaptureui"></a>Capturar un vídeo con CameraCaptureUI

Para capturar un vídeo, crea un objeto [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) nuevo. El uso de la propiedad [**VideoSettings**](https://msdn.microsoft.com/library/windows/apps/br241059) del objeto puede especificar las propiedades del vídeo devuelto, como el formato de imagen del vídeo.

Realiza una llamada a [**CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057) y especifica [**Video**](https://msdn.microsoft.com/library/windows/apps/br241059) para especificar que se debe capturar un vídeo. El método devuelve una instancia [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que contiene el vídeo si la captura se realiza correctamente. Si el usuario cancela la captura, el objeto devuelto es null.

[!code-cs[CaptureVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCaptureVideo)]

Qué hacer con el archivo de vídeo capturado depende del escenario para la aplicación. El resto de este artículo muestra cómo crear rápidamente una composición multimedia de uno o más vídeos capturados y mostrarla en la interfaz de usuario.

Primero, agrega un control [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926), en el que se mostrará la composición de vídeo en la página XAML.

[!code-xml[MediaElement](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetMediaElement)]

Agrega los espacios de nombres [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565) y [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) al proyecto.


[!code-cs[UsingMediaComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingMediaComposition)]

Declara las variables de miembro para un objeto [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646) y una clase [**MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn282716) que quieras mantener en el ámbito durante la duración de la página.

[!code-cs[DeclareMediaComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetDeclareMediaComposition)]

Una vez, antes de capturar vídeos, debes crear una nueva instancia de la clase **MediaComposition**.

[!code-cs[InitComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetInitComposition)]

Con el archivo de vídeo devuelto desde la interfaz de usuario de captura de cámara, crea una clase [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596) nueva llamando a [**MediaClip.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652607). Agrega el clip multimedia a la colección [**Clips**](https://msdn.microsoft.com/library/windows/apps/dn652648) de la composición.

Llama a [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674) para crear el objeto **MediaStreamSource** desde la composición.

[!code-cs[AddToComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetAddToComposition)]

Finalmente, define el origen de la secuencia para usar el método [**SetMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn299029) del elemento multimedia para mostrar la composición en la interfaz de usuario.

[!code-cs[SetMediaElementSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetMediaElementSource)]

Puedes continuar capturando clips de vídeo y agregarlos a la composición. Para más información sobre las composiciones multimedia, consulta [Composiciones multimedia y edición](media-compositions-and-editing.md).

> [!NOTE] 
> Este artículo está orientado a desarrolladores de Windows 10 que programan aplicaciones para la Plataforma universal de Windows (UWP). Si estás desarrollando para Windows 8.x o Windows Phone 8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 
 

 





