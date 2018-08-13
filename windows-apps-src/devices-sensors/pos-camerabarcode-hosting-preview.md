---
author: TerryWarwick
title: Vista previa de hospedaje para escáneres de código de barras basados en cámara
description: Aprende a hospedar en tu aplicación una vista previa para escáneres de código de barras basados en cámara
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 4e5765a725ad99a1092ad8c56cef674ec6210a2c
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
ms.locfileid: "1833356"
---
# <a name="hosting-a-camera-barcode-scanner-preview-in-your-application"></a>Hospedaje de vista previa para escáneres de código de barras basados en cámara en tu aplicación
## <a name="step-1-setup-your-camera-preview"></a>Paso 1: configura la vista previa de cámara
El primer paso para agregar una vista previa a la aplicación para el escáner de código de barras basado en cámara se puede lograr siguiendo las instrucciones del tema [Mostrar la vista previa de la cámara](../audio-video-camera/simple-camera-preview-access.md).  Después de completar este paso, vuelve a este tema para modificaciones específicas sobre el escáner de código de barras basado en cámara.

## <a name="step-2-update-capability-declarations"></a>Paso 2: actualizar las declaraciones de funcionalidad
Para impedir que los usuarios reciban el aviso consentimiento para el micrófono puedes excluir esto de las funcionalidades que se enumeran en el manifiesto de la aplicación.

1. En Microsoft Visual Studio, en el **Explorador de soluciones**, abre el diseñador para el manifiesto de la aplicación haciendo doble clic en el elemento **package.appxmanifest**.
2. Selecciona la pestaña **Funcionalidades**.
3. Desactiva la casilla **Microphone**

 ## <a name="step-3-add-additional-using-directive-for-media-capture"></a>Paso 3: agrega directivas de uso adicionales para la captura multimedia

```Csharp
using Windows.Media.Capture;
```

## <a name="step-4-set-up-your-mediacapture-initialization-settings"></a>Paso 4: configura los ajustes de inicialización de MediaCapture
El siguiente ejemplo inicializa [**MediaCaptureInitializationSettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings). 

```Csharp
 private void InitCaptureSettings()
{
    _captureInitSettings = new MediaCaptureInitializationSettings();
    _captureInitSettings.VideoDeviceId = ClaimedBarcodeScanner.VideoDeviceId;
    _captureInitSettings.StreamingCaptureMode = StreamingCaptureMode.Video;
    _captureInitSettings.PhotoCaptureSource = PhotoCaptureSource.VideoPreview;
    
    if (_deviceList.Count > 0)
    {
        _captureInitSettings.VideoDeviceId = _deviceList[0].Id;
    }
}
```
## <a name="step-5-associate-your-mediacapture-object-with-the-camera-barcode-scanner"></a>Paso 5: asocia el objeto MediaCapture con el escáner de código de barras basado en cámara
Reemplaza el mediaCapture.InitializeAsync() existente en *StartPreviewAsync()* con lo siguiente:

```Csharp
try
    {

        mediaCapture = new MediaCapture();
        await mediaCapture.InitializeAsync(InitCaptureSettings());

        displayRequest.RequestActive();
        DisplayInformation.AutoRotationPreferences = DisplayOrientations.Landscape;
    }
```

> [!TIP]
> Consulta [Mostrar la vista previa de cámara](https://docs.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access#add-capability-declarations-to-the-app-manifest) para obtener temas más avanzados sobre el alojamiento de una vista previa de cámara en tu aplicación.