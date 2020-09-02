---
ms.assetid: 05E418B4-5A62-42BD-BF66-A0762216D033
description: En este tema se muestra cómo obtener un solo fotograma de vista previa de la secuencia de vista previa de captura multimedia.
title: Obtener un fotograma de vista previa
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1a688ade1e8907cb0de0683df0751d1eebef4ed7
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362628"
---
# <a name="get-a-preview-frame"></a>Obtener un fotograma de vista previa


En este tema se muestra cómo obtener un solo fotograma de vista previa de la secuencia de vista previa de captura multimedia.

> [!NOTE] 
> Este artículo se basa en los conceptos y el código analizados en [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), donde se describen los pasos para implementar la captura básica de fotos y vídeo. Se recomienda que te familiarices con el patrón de captura de multimedia básico de ese artículo antes de pasar a escenarios de captura más avanzados. En el código de este artículo se supone que la aplicación ya tiene una instancia de MediaCapture que se ha inicializado correctamente y que tiene un objeto [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) con una secuencia de vista previa de vídeo activa.

Además de los espacios de nombres necesarios para realizar la captura multimedia básica, necesitarás el siguiente espacio de nombres para capturar un marco de vista previa.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetPreviewFrameUsing":::

Al solicitar un marco de vista previa, puedes especificar el formato en el que quieres recibir el marco creando un objeto [**VideoFrame**](/uwp/api/Windows.Media.VideoFrame) con el formato que quieras. En este ejemplo se crea un marco de vídeo que tiene la misma la resolución que la secuencia de vista previa; para ello, se ha de llamar al método [**VideoDeviceController.GetMediaStreamProperties**](/uwp/api/windows.media.devices.videodevicecontroller.getmediastreamproperties) y se debe especificar la enumeración [**MediaStreamType.VideoPreview**](/uwp/api/Windows.Media.Capture.MediaStreamType) para solicitar las propiedades de la secuencia de vista previa. El ancho y alto de la secuencia de vista previa se usa para crear el nuevo marco de vídeo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCreateFormatFrame":::

Si el objeto [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) se inicializa y tiene una secuencia de vista previa activa, llama a [**GetPreviewFrameAsync**](/uwp/api/windows.media.capture.mediacapture.getpreviewframeasync) para obtener una secuencia de vista previa. Pasa el marco de vídeo creado en el último paso, para especificar el formato del marco devuelto.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetPreviewFrameAsync":::

Consigue una representación de [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) del fotograma de vista previa mediante el acceso a la propiedad [**SoftwareBitmap**](/uwp/api/windows.media.videoframe.softwarebitmap) del objeto [**VideoFrame**](/uwp/api/Windows.Media.VideoFrame). Para obtener información sobre cómo guardar, cargar y modificar mapas de bits de software, consulta [Imágenes](imaging.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetPreviewBitmap":::

Asimismo, también puedes obtener una representación de [**IDirect3DSurface**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) del fotograma de vista previa si quieres usar la imagen con las API de Direct3D.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetPreviewSurface":::

> [!IMPORTANT]
> Es posible que la propiedad [**SoftwareBitmap**](/uwp/api/windows.media.videoframe.softwarebitmap) o la propiedad [**Direct3DSurface**](/uwp/api/windows.media.videoframe.direct3dsurface) del valor devuelto de **VideoFrame** sea nula; todo depende de cómo hayas llamado a **GetPreviewFrameAsync** y del tipo de dispositivo en el que se ejecute la aplicación.

> - Si llamas a la sobrecarga de [**GetPreviewFrameAsync**](/uwp/api/windows.media.capture.mediacapture.getpreviewframeasync) que acepta un argumento **VideoFrame**, el valor de **VideoFrame** devuelto tendrá una propiedad **SoftwareBitmap** que no será nula y una propiedad **Direct3DSurface** que sí lo será.
> - Si llamas a la sobrecarga de un método [**GetPreviewFrameAsync**](/uwp/api/windows.media.capture.mediacapture.getpreviewframeasync) que no tenga ningún argumento en un dispositivo que use una superficie de Direct3D para representar el marco internamente, la propiedad **Direct3DSurface** no será nula y la propiedad **SoftwareBitmap** sí lo será.
> - Si llamas a la sobrecarga de un método [**GetPreviewFrameAsync**](/uwp/api/windows.media.capture.mediacapture.getpreviewframeasync) que no tenga ningún argumento en un dispositivo que no use una superficie de Direct3D para representar el marco internamente, la propiedad **SoftwareBitmap** no será nula y la propiedad **Direct3DSurface** sí lo será.

La aplicación debe comprobar siempre si hay un valor "null" antes de intentar operar con los objetos devueltos por las propiedades **SoftwareBitmap** o **Direct3DSurface**.

Cuando hayas terminado con el marco de vista previa, asegúrate de llamar a su método [**Close**](/uwp/api/windows.media.videoframe.close) (previsto para Dispose en C#), para liberar los recursos usados por el marco. Igualmente, también puedes usar el patrón **using**, que elimina automáticamente el objeto.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCleanUpPreviewFrame":::

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 
