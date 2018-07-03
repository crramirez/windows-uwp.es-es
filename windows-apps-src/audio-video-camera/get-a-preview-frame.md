---
author: drewbatgit
ms.assetid: 05E418B4-5A62-42BD-BF66-A0762216D033
description: En este tema se muestra cómo obtener un solo fotograma de vista previa de la secuencia de vista previa de captura multimedia.
title: Obtener un fotograma de vista previa
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4c1d346a0ae3cd67fb1242e16de308b54520d435
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842712"
---
# <a name="get-a-preview-frame"></a>Obtener un fotograma de vista previa


En este tema se muestra cómo obtener un solo fotograma de vista previa de la secuencia de vista previa de captura multimedia.

> [!NOTE] 
> Este artículo se basa en los conceptos y el código analizados en [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), donde se describen los pasos para la implementación de la captura básica de fotos y vídeo. Se recomienda que te familiarices con el patrón de captura de multimedia básico de ese artículo antes de pasar a escenarios de captura más avanzados. Para usar el código de este artículo, se presupone que la aplicación ya tiene una instancia de MediaCapture inicializada correctamente y una clase [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) con una secuencia de vista previa de vídeo activa.

Además de los espacios de nombres necesarios para realizar la captura multimedia básica, necesitarás el siguiente espacio de nombres para capturar un marco de vista previa.

[!code-cs[PreviewFrameUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewFrameUsing)]

Al solicitar un marco de vista previa, puedes especificar el formato en el que quieres recibir el marco creando un objeto [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917) con el formato que quieras. En este ejemplo se crea un marco de vídeo que tiene la misma la resolución que la secuencia de vista previa; para ello, se ha de llamar al método [**VideoDeviceController.GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211995) y se debe especificar la enumeración [**MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640) para solicitar las propiedades de la secuencia de vista previa. El ancho y alto de la secuencia de vista previa se usa para crear el nuevo marco de vídeo.

[!code-cs[CreateFormatFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateFormatFrame)]

Si el objeto [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) se inicializa y tiene una secuencia de vista previa activa, llama a [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926711) para obtener una secuencia de vista previa. Pasa el marco de vídeo creado en el último paso, para especificar el formato del marco devuelto.

[!code-cs[GetPreviewFrameAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewFrameAsync)]

Consigue una representación de [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) del fotograma de vista previa mediante el acceso a la propiedad [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn930926) del objeto [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917). Para obtener información sobre cómo guardar, cargar y modificar mapas de bits de software, consulta [Imágenes](imaging.md).

[!code-cs[GetPreviewBitmap](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewBitmap)]

Asimismo, también puedes obtener una representación de [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505) del fotograma de vista previa si quieres usar la imagen con las API de Direct3D.

[!code-cs[GetPreviewSurface](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewSurface)]

> [!IMPORTANT]
> Es posible que la propiedad [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn930926) o la propiedad [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn930920) del valor devuelto de **VideoFrame** sea nula; todo depende de cómo hayas llamado a **GetPreviewFrameAsync** y del tipo de dispositivo en el que se ejecute la aplicación.

> - Si llamas a la sobrecarga de [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926713) que acepta un argumento **VideoFrame**, el valor de **VideoFrame** devuelto tendrá una propiedad **SoftwareBitmap** que no será nula y una propiedad **Direct3DSurface** que sí lo será.
> - Si llamas a la sobrecarga de un método [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926712) que no tenga ningún argumento en un dispositivo que use una superficie de Direct3D para representar el marco internamente, la propiedad **Direct3DSurface** no será nula y la propiedad **SoftwareBitmap** sí lo será.
> - Si llamas a la sobrecarga de un método [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926712) que no tenga ningún argumento en un dispositivo que no use una superficie de Direct3D para representar el marco internamente, la propiedad **SoftwareBitmap** no será nula y la propiedad **Direct3DSurface** sí lo será.

La aplicación debe comprobar siempre si hay un valor "null" antes de intentar operar con los objetos devueltos por las propiedades **SoftwareBitmap** o **Direct3DSurface**.

Cuando hayas terminado con el marco de vista previa, asegúrate de llamar a su método [**Close**](https://msdn.microsoft.com/library/windows/apps/dn930918) (previsto para Dispose en C#), para liberar los recursos usados por el marco. Igualmente, también puedes usar el patrón **using**, que elimina automáticamente el objeto.

[!code-cs[CleanUpPreviewFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpPreviewFrame)]

## <a name="related-topics"></a>Artículos relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




