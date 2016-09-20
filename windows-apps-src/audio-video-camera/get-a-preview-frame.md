---
author: drewbatgit
ms.assetid: 05E418B4-5A62-42BD-BF66-A0762216D033
description: "En este tema se muestra cómo obtener un marco de vista previa de la secuencia de vista previa de captura multimedia."
title: Obtener un marco de vista previa
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c512ec92272ab03cfd8e91602018f09ef8225652

---

# Obtener un marco de vista previa

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En este tema se muestra cómo obtener un marco de vista previa de la secuencia de vista previa de captura multimedia.

**Nota**  
Este artículo se basa en los conceptos y el código analizados en [Capturar fotografías y vídeos con MediaCapture](capture-photos-and-video-with-mediacapture.md), donde se describen los pasos para la implementación de la captura básica de fotografías y vídeos. Se recomienda que te familiarices con el patrón de captura de multimedia básico de ese artículo antes de pasar a escenarios más avanzados de captura. Para usar el código de este artículo, se presupone que la aplicación ya tiene una instancia de MediaCapture inicializada correctamente y una clase [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) con una secuencia de vista previa de vídeos activa.

Además de los espacios de nombres necesarios para realizar la captura multimedia básica, necesitarás el siguiente espacio de nombres para capturar un marco de vista previa.

[!code-cs[PreviewFrameUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewFrameUsing)]

Al solicitar un marco de vista previa, puedes especificar el formato en el que quieres recibir el marco creando un objeto [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917) con el formato que quieras. En este ejemplo se crea un marco de vídeo que tiene la misma la resolución que la secuencia de vista previa; para ello, se ha de llamar al método [**VideoDeviceController.GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211995) y se debe especificar la enumeración [**MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640) para solicitar las propiedades de la secuencia de vista previa. El ancho y alto de la secuencia de vista previa se usa para crear el nuevo marco de vídeo.

[!code-cs[CreateFormatFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateFormatFrame)]

Si el objeto [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) se inicializa y tiene una secuencia de vista previa activa, llama a [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926711) para obtener una secuencia de vista previa. Pasa el marco de vídeo creado en el último paso, para especificar el formato del marco devuelto.

[!code-cs[GetPreviewFrameAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewFrameAsync)]

Consigue una representación [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) del marco de vista previa mediante el acceso a la propiedad [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn930926) del objeto [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917). Para obtener información sobre cómo guardar, cargar y modificar mapas de bits de software, consulta [Imágenes](imaging.md).

[!code-cs[GetPreviewBitmap](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewBitmap)]

Asimismo, también puedes obtener una representación [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505) del marco de vista previa si quieres usar la imagen con las API de Direct3D.

[!code-cs[GetPreviewSurface](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewSurface)]

**Importante**  
Es posible que la propiedad [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn930926) o la propiedad [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn930920) del valor devuelto **VideoFrame** sea nula; todo depende de cómo hayas llamado al método **GetPreviewFrameAsync** y del tipo de dispositivo en el que se ejecuta la aplicación.

-   Si llamas a la sobrecarga de un método [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926713) que acepte un argumento **VideoFrame**, la clase **VideoFrame** tendrá una propiedad **SoftwareBitmap** que no será nula y una propiedad **Direct3DSurface** que sí lo será.
-   Si llamas a la sobrecarga de un método [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926712) que no tenga ningún argumento en un dispositivo que use una superficie de Direct3D para representar el marco internamente, la propiedad **Direct3DSurface** no será nula y la propiedad **SoftwareBitmap** sí lo será.
-   Si llamas a la sobrecarga de un método [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926712) que no tenga ningún argumento en un dispositivo que no use una superficie de Direct3D para representar el marco internamente, la propiedad **SoftwareBitmap** no será nula y la propiedad **Direct3DSurface** sí lo será.

La aplicación debe comprobar siempre si hay un valor "null" antes de intentar operar con los objetos devueltos por las propiedades **SoftwareBitmap** o **Direct3DSurface**.

Cuando hayas terminado con el marco de vista previa, asegúrate de llamar a su método [**Close**](https://msdn.microsoft.com/library/windows/apps/dn930918) (previsto para Dispose en C#), para liberar los recursos usados por el marco. Igualmente, también puedes usar el patrón **using**, que elimina automáticamente el objeto.

[!code-cs[CleanUpPreviewFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpPreviewFrame)]

## Temas relacionados

* [Capturar fotografías y vídeos con MediaCapture](capture-photos-and-video-with-mediacapture.md)
 

 







<!--HONumber=Jun16_HO4-->


